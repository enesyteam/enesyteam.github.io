---
layout: post
title:  "Xref một bản vẽ sử dụng đường dẫn tương đối (Relative Path) bằng .NET"
desc: "Bài "
keywords: "transaction,autocad,enesy"
date: 2016-10-11
categories: [C#]
tags: [autocad,xref,relative path,.NET]
icon: fa-bookmark-o
---

## Đặt vấn đề

Đối với các bản vẽ có sử dụng tham chiếu ngoài `XREF`, một vấn đề thường hay gặp phải đó là `AutoCAD` không tìm thấy file mà bạn đã `XREF` vào bản vẽ mỗi khi di chuyển nó đến một thư mục khác. Lý do đơn giản và dễ hiểu của việc này đó là `AutoCAD` đã lưu đường dẫn file `XREF` dưới dạng địa chỉ tuyện đối, có dạng: `C:\example\drawing1.dwg`. Vì vậy, mỗi khi khởi động bản vẽ, `AutoCAD` sẽ tìm và load file `XREF` theo địa chỉ đã được lưu. Nếu file đã bị di chuyển thi đương nhiên là `AutoCAD` sẽ không tìm thấy, bạn sẽ không nhìn thấy file `XREF` trong bản vẽ của mình nữa.

<figure class="one">
	<img src="/static/img/blog/csharp/2016-10-31-1.jpg" alt="">
</figure>

### Giải pháp

Thay vì sử dụng các địa chỉ tuyệt đối, chúng ta sẽ sử dụng các địa chỉ tương đối (`relative path`) đối với đường dẫn các file `XREF`, tuy nhiên `AutoCAD` không cho phép chúng ta trực tiếp sửa các đường dẫn này trong cửa sổ `EXTERNAL REFERENCE`. May mắn thay là chúng ta có thể can thiệp được điều này thông qua `.NET`.

Đầu tiên chúng ta sẽ tạo ra một `class` có tên `Upgrader`, được kế thừa từ `interface` `IDisposable`, lớp này sẽ đóng vai trò trợ giúp trong việc đảm bảo đối tượng đã được mở để ghi (`write`), và `downgdading` (chấm dứt) nó khi trạng thái mở này được hoàn tất. Điều này cũng tương tự như việc sử dụng phương thức khóa tài liệu (`document lock`) thông qua câu lệnh `using()`.

```cs
	// Simple helper to make sure an object is "open for write", downgrading
    // its open status when finished. It's intended to be used in the same way
    // as a document lock from a using() statement.
    public class Upgrader : IDisposable
    {
        private bool _upgraded;
        private DBObject _object;
        public Upgrader(DBObject o)
        {
            _object = o;
            _upgraded = o.IsReadEnabled;
            if (_upgraded)
                o.UpgradeOpen();
        }
        public void Dispose()
        {
            Dispose(true);
            GC.SuppressFinalize(this);
        }
        protected virtual void Dispose(bool disposing)
        {
            if (disposing)
            {
                if (_upgraded && _object != null)
                    _object.DowngradeOpen();
                _object = null;
            }
        }
    }
```

Mã nguồn chính của chúng ta như sau:

```cs
	public static class Extensions
    {
        /// <summary>
        /// Generates a relative path from one string to another.
        /// </summary>
        /// <param name="to">The path to which to return a relative path.</param>
        /// <returns>The relative path between this one and the destination.</returns>
        public static string RelativePathTo(this string from, string to)
        {
            var fromUri = new Uri(from);
            var toUri = new Uri(to);
            // Convert to URIs for the sake of the relative path determination
            var relUri = fromUri.MakeRelativeUri(toUri);
            var relPath = Uri.UnescapeDataString(relUri.ToString());
            // Then change back
            return relPath.Replace('/', Path.DirectorySeparatorChar);
        }
        /// <summary>
        /// Changes the path of an xref's block definition to have a relative path.
        /// </summary>
        /// <param name="root">Path from which to create the relative path.</param>
        /// <returns>Whether the path was changed - only fails for non-xrefs.</returns>
        public static bool ChangePathToRelative(this BlockTableRecord btr, string root)
        {
            var ret = false;
            if (btr.IsFromExternalReference)
            {
                using (new Upgrader(btr))
                {
                    btr.PathName = root.RelativePathTo(btr.PathName);
                    ret = true;
                }
            }
            return ret;
        }
        /// <summary>
        /// Attaches the specified Xref to the current space in the current drawing.
        /// </summary>
        /// <param name="path">Path to the drawing file to attach as an Xref.</param>
        /// <param name="pos">Position of Xref in WCS coordinates.</param>
        /// <param name="name">Optional name for the Xref.</param>
        /// <returns>Whether the attach operation succeeded.</returns>
        public static bool XrefAttachAndInsert(this Database db, string path, Point3d pos,string name = null, bool overlay = false)
        {
            var ret = false;
            if (!File.Exists(path))
                return ret;
            if (String.IsNullOrEmpty(name))
                name = Path.GetFileNameWithoutExtension(path);
            // We'll collect any xref definitions that need reloading after our
            // transaction (there should be at most one)
            var xIds = new ObjectIdCollection();
            try
            {
                using (var tr = db.TransactionManager.StartTransaction())
                {
                    // Attach or overlay the Xref - add it to the database's block table
                    var xId =
                      overlay ? db.OverlayXref(path, name) : db.AttachXref(path, name);
                    if (xId.IsValid)
                    {
                        // Open the newly created block, so we can get its units
                        var xbtr = (BlockTableRecord)tr.GetObject(xId, OpenMode.ForRead);
                        // Get the path of the current drawing
                        var loc = Path.GetDirectoryName(db.Filename);
                        if (xbtr.ChangePathToRelative(loc))
                        {
                            xIds.Add(xId);
                        }
                        // Determine the unit conversion between the xref and the target
                        // database
                        var sf = UnitsConverter.GetConversionFactor(xbtr.Units, db.Insunits);
                        // Create the block reference and scale it accordingly
                        var br = new BlockReference(pos, xId);
                        br.ScaleFactors = new Scale3d(sf);
                        // Add the block reference to the current space and the transaction
                        var btr = (BlockTableRecord)tr.GetObject(db.CurrentSpaceId, OpenMode.ForWrite);
                        btr.AppendEntity(br);
                        tr.AddNewlyCreatedDBObject(br, true);
                        ret = true;
                    }
                    tr.Commit();
                }
                // If we modified our xref's path, let's reload it
                if (xIds.Count > 0)
                {
                    //db.ReloadXrefs(xIds);
                    System.Windows.Forms.MessageBox.Show("You need reload Xrefs!");
                }
            }
            catch (Autodesk.AutoCAD.Runtime.Exception)
            { }
            return ret;
        }
    }
```

Và cuối cùng, đây là mã lệnh của `Commands`:

```cs
	public static class Commands
    {
        [CommandMethod("XAO")]
        public static void XrefAttachAtOrigin()
        {
            XrefAttachOrOverlayAtOrigin();
        }
        [CommandMethod("XOO")]
        public static void XrefOverlayAtOrigin()
        {
            XrefAttachOrOverlayAtOrigin(true);
        }
        private static void XrefAttachOrOverlayAtOrigin(bool overlay = false)
        {
            var doc = Application.DocumentManager.MdiActiveDocument;
            if (doc == null)
                return;
            var db = doc.Database;
            var ed = doc.Editor;
            // Ask the user to specify a file to attach
            var opts = new PromptOpenFileOptions("Select Reference File");
            opts.Filter = "Drawing (*.dwg)|*.dwg";
            var pr = ed.GetFileNameForOpen(opts);
            if (pr.Status == PromptStatus.OK)
            {
                // Overlay the specified file and insert it at the origin
                var res =db.XrefAttachAndInsert(pr.StringResult, Point3d.Origin, null, overlay);
                ed.WriteMessage("External reference {0}{1} at the origin.", res ? "" : "not ", overlay ? "overlaid" : "attached");
            }
        }
    }
```

## Sử dụng với EnesyCAD

Tiện ích này đã có sẵn trong `EnesyCAD`, bạn có thể sử dụng `EnesyCAD` với các lệnh `XAO` và `XOO`.