---
layout: post
title:  "(.NET) Xây dựng một lớp cơ sở nhằm đơn giản hóa quá trình thêm đối tượng vào AutoCAD"
desc: "Khi lập trình AutoCAD với các ngôn ngữ .NET, một thao tác mà bạn chắc chắn không thể tránh khỏi là mở Transaction, sau đó thì đóng Transaction, nhưng không phải lúc nào bạn cũng nhớ các thao tác này"
keywords: "transaction,autocad,enesy"
date: 2016-10-10
categories: [C#]
tags: [autocad,transaction,.NET]
icon: fa-bookmark-o
---

### Đặt vấn đề

Khi lập trình `AutoCAD` với các ngôn ngữ `.NET`, một điều bắt buộc khi bạn muốn thêm đối tượng vào bản vẽ đó là: Mở `Transaction`, thêm đối tượng vào `Database`, thêm nó vào `Transaction`, cuối cùng là `Commit Transaction` này, lúc đó đối tượng mới được thêm vào bản vẽ.

Vấn đề thứ nhất: Không phải lúc nào bạn cũng nhớ gọi phương thức `Commit` của `Transaction`. Và nếu chẳng may quên đóng `Transaction` thì lỗi sau sẽ xuất hiện khi bạn thực thi lệnh:

<figure class="one">
	<img src="/static/img/blog/csharp/2016-10-10-1.jpg" alt="">
	<figcaption>Lỗi xuất hiện do bạn quên đóng `Transaction`.</figcaption>
</figure>

Điều này thực sự tai hại, vì lỗi này sẽ khiến bạn phải đóng `AutoCAD` mà không thể lưu các bản vẽ hiện tại. Hơn nữa, trong các `Transaction` lồng nhau (`Nested Transaction`) thì việc quên đóng một `Transaction` nào đó là rất dễ xảy ra.

Ngoài ra, một lỗi phổ biến khác cũng thường hay gặp phải khi mở `BlockTableRecord` để thêm đối tượng vào đó là bạn chỉ mở nó dưới dạng `ReadOnly` (tức là chỉ mở ra để đọc dữ liệu mà không có quyền ghi dữ liệu vào). Hãy xem đoạn mã sau đây:

```cs
BlockTableRecord btr = Transaction.GetObject(acDb.CurrentSpaceId, OpenMode.ForRead) as BlockTableRecord;
```

Một điều đặc biệt cần chú ý là không phải lúc nào bạn cũng nên mở `BlockTableRecord` dưới dạng `ForWrite` bởi vì điều này tuy sẽ không ảnh hưởng nhiều đến tốc độ thực thi của lệnh trong các tiến trình đơn lẻ, nhưng hãy tưởng tượng bạn có một tiến trình gồm nhiều tiến trình con, trong đó các tiến trình con này đều mở `BlockTableRecord` dưới dạng `ForWrite` thì thời gian thực thi sẽ tăng lên nhiều nếu nó thực sự không cần thiết phải mở dưới dạng `ForWrite`.

### Giải pháp

Thay vì phải luôn luôn để ý đến các điểm mấu chốt như đã nói trên, tại sao chúng ta không viết một `class` cơ sở, trong đó có chứa một phương thức thêm đối tượng vào bản vẽ mà nó thực hiện các khâu này một cách chuẩn mực. Như vậy chúng ta sẽ không cần phải để ý đến việc phải thêm đối tượng vào bản vẽ như thế nào, thay vào đó chúng ta chỉ việc gọi phương thức thêm đối tượng của lớp cơ sở. Để dễ hình dung từng bước tạo một `class` như vậy, mình sẽ bắt đầu bằng việc viết giao diện (`interface`) cho nó như sau:

```cs
using System;
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.DatabaseServices;

namespace EnesyCAD
{
    public interface IFigure
    {
        Document Drawing { get; set; }
        Transaction Transaction { get; set; }
        void Append(Document drawing);
}
```
Để ý giao diện này ta sẽ thấy nó bao gồm hai `Properties` là `Drawing` và `Transaction`, một phương thức là `Append`. Một đối tượng `Figure` kế thừa từ giao diện `IFigure` sẽ có dạng như sau:

```cs
using System;
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.DatabaseServices;

namespace EnesyCAD
{
    public class FigureBase :  IFigure
    {
        public Document Drawing { get; set; }
        public Transaction Transaction { get; set; }
        public BlockTableRecord BlockTableRecord { get; set; }
        public virtual void Append(Document drawing)
        { 
        	// Các lệnh thêm đối tượng vào bản vẽ ở đây
        }
    }
}

```
Lớp mà chúng ta muốn tạo để dùng làm lớp cơ sở cho các lớp đối tượng `AutoCAD` sẽ có tên là `CompositeFigure`, kế thừa từ lớp cơ sở là `FigureBase`. Lý do mà mình tạo ra lớp `CompositeFigure` ở đây là mình muốn nó có thể chứa được rất nhiều dạng đối tượng của `AutoCAD`, điều này có nghĩa là nó không phải là một đối tượng đơn lẻ (như 1 `Line`, 1 `DbText`, ...) mà có thể là tổ hợp của chúng. Như vậy nó sẽ phải chứa một `Properties` có dạng một `Collection`, ở đây mình đặt tên là `Children`.

```cs
using System;
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.DatabaseServices;
using System.Collections.Generic;
using System.ComponentModel;
using Autodesk.AutoCAD.Geometry;

namespace EnesyCAD
{
    public class CompositeFigure : FigureBase
    {
        List<Entity> _children = null;
        public List<Entity> Children
        {
            get { return _children; }
            set { _children = value;
            OnPropertyChanged("Children");
            }
        }
        public CompositeFigure()
        {
            Children = new List<Entity>();
        }
        public virtual Point2d InsertPoint { get; set; }
        public override void Append(Document drawing)
        {
            this.Drawing = drawing;
            Database acDb = Drawing.Database;
            Transaction = Drawing.TransactionManager.StartTransaction();
            BlockTableRecord = Transaction.GetObject(acDb.CurrentSpaceId, OpenMode.ForWrite) as BlockTableRecord;
            foreach (Entity f in Children)
            {
                BlockTableRecord.AppendEntity(f);
                Transaction.AddNewlyCreatedDBObject(f, true);
            }
            Transaction.Commit();
        }
    }
}

```

### Ứng dụng

Giả sử bạn muốn thêm một đối tượng phức hợp đơn giản gồm hai đoạn thẳng vào bản vẽ, đoạn mã này sẽ được viết như sau:

```cs
using System;
using Autodesk.AutoCAD.ApplicationServices;
using Autodesk.AutoCAD.DatabaseServices;
using System.Collections.Generic;
using Autodesk.AutoCAD.Runtime;

namespace EnesyCAD
{
    public static class Test
    {
        [CommandMethod("TEST")]
        public  static void TestAdd()
        {
            Document doc = Application.DocumentManager.MdiActiveDocument;
            CompositeFigure com = new CompositeFigure();
            Line l1 = new Line(new Autodesk.AutoCAD.Geometry.Point3d(0, 0, 0),
                new Autodesk.AutoCAD.Geometry.Point3d(100, 0, 0));
            Line l2 = new Line(new Autodesk.AutoCAD.Geometry.Point3d(100, 0, 0),
    			new Autodesk.AutoCAD.Geometry.Point3d(100, 100, 0));
            com.Children.Add(l1);
            com.Children.Add(l2);
            com.Append(doc);
        }
    }
}
```

Hãy xem đoạn mã này: Chúng ta chỉ việc tạo ra 2 đối tượng hình học là `l1` và `l2`, sau đó thêm vào `Properties` của lớp cơ sở là `CompositeFigure` và cuối cùng là gọi phương thức `Append` của nó. Các công việc như tạo `Transaction`, mở `BlockTableRecord` để ghi, thêm đối tượng vào `Transaction`, thêm đối tượng vào `Database`, và `Commit` `Transaction` sẽ được thực hiện ở lớp cơ sở `CompositeFigure`. Chúng ta không cần phải quan tâm đến chúng nữa.

Các bạn thấy giải pháp này như thế nào? Hãy thử áp dụng và xem hiệu quả của nó nhé! Xin chào và hẹn gặp lại các bạn trong các bài sau!


