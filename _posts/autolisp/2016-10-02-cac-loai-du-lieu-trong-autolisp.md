---
layout: post
title:  "Các loại dữ liệu AutoLISP"
desc: "Tìm hiểu về các loại dữ liệu được sử dụng trong AutoLISP"
keywords: "autolisp,enesy"
date: 2016-10-02
categories: [AutoLISP]
tags: [autolisp]
icon: fa-file-code-o
---

## Các loại dữ liệu Trong AutoLISP

Các biểu thức `AutoLISP` được xử lý theo thứ tự và loại dữ liệu của mã trong các dấu ngoặc đơn. Trước khi bạn hoàn toàn có thể sử dụng `AutoLISP`, bạn phải hiểu những điểm khác biệt giữa các loại dữ liệu và cách sử dụng chúng.

### Số nguyên

Các số nguyên là các số không chứa dấu thập phân. Các số nguyên trong `AutoLISP` là các số được đánh dấu `32 bit` với các giá trị nằm trong khoảng từ `-2,147,483,647` đến `2,147,483,648`. Tuy nhiên, chú ý tằng hàm `getint` chỉ chấp nhận các số `16 bit` nằm trong dãy từ `-32767` đến `+32678`. Khi bạn sử dụng một số nguyên trong một biểu thức `AutoLISP`, giá trị đó được gọi là một hằng. Các số chẳng hạn như `2`, `56` và `1.200,196` là các số nguyên `AutoLISP` hợp lệ.
Nếu bạn nhập một số lớn hơn số nguyen tối đa cho phép (gây ra sự tràn số nguyên), `AutoLISP` chuyển đổi số nguyên thành một số thực. Tuy nhiên, nếu bạn thực hiện một phép tính số học trên hai số nguyên hợp lện và kết quả lớn hơn số nguyên tối đa cho phép, số vừa tạo ra sẽ không hợp lệ. Các ví dụ sau đây minh họa cách `AutoLISP` xử lý sự tràn số nguyên.
Giá trị nguyên dương lớn nhất giữ lại giá trị được xác định của nó:

```
_$2147483647
2147483647
```

Nếu bạn nhập một số nguyên lớn hơn giá trị lớn nhất cho phép, `AutoLISP` cho ra giá trị dưới dạng một số thực:

```
_$2147483648
2.14748e+009
```

Một phép tính số học gồm hai số nguyên hợp lệ, nhưng dẫn đến sự tràn số nguyên, tạo ra một kết quả không hợp lệ:

```
_$(+ 2147483646 3)
-2147483647
```

Trong ví dụ này, kết quả rõ ràng không hợp lệ, vì phép cộng của hai số dương tạo ra một số âm. Nhưng chú ý cách phép tính sau đây tạo ra một kết quả hợp lệ:

```
_$(+ 2147483648 2)
2.14748e+009
```

Trong trường hợp này, `AutoLISP` chuyển đổi số 2147483648 thành một số thực hợp lệ trước khi cộng 2 với số này. Kết quả là một số thực hợp lệ.

Giá trị số nguyên âm lớn nhất giữ lại giá trị đã xác định của nó:

```
_$-2147483647
-2147483647
```

Nếu bạn nhập một số nguyên âm lớn hơn giá trị âm lớn nhất cho phép, `AutoLISP` cho ra giá trị dưới dạng một số thực:

```
_$-2147483648
-2.14748e+009
```

Phép tính sau đây kết thúc thành công bởi vì `AutoLISP` chuyển đổi trước số nguyên âm thành một số thực hợp lệ:

```
_$(-2147483648 1)
-2.14748e+009
```

### Số thực

Một số thực là một số chứa một dấu thập phân. Các số thực được lưu trữ theo dạng dấu động có độ chính xác kép, cung cấp tối thiểu 14 chữ số chính xác có nghĩa. Chú ý rằng `VLISP` không hiển thị cho bạn chữ số có nghĩa.

Các số thực có thể được biểu diễn theo ký hiệu khoa học, ký hiệu này có một `e` hoặc `E` tùy ý được theo sáu bởi số mũ của số (ví dụ, `0.000041` tương tự như `4.1e-6`). Các số như `3.1`, `0.23`, `56.123`, và `21,000,000.0` đều là các số thực `AutoLISP` hợp lệ.

### Chuỗi

Một chuỗi là một nhóm các ký tự được bao quanh bởi các dấu trích dẫn.Trong các chuỗi được trích dẫn, ký tự `backslash` (`\`) cho phép đưa vào các ký tự điều khiển (hoặc các mã thoát). Khi bạn sử dụng một chuỗi được trích dẫn trong một biểu thức `AutoLISP`, giá trị đó được gọi là một chuỗi thực hiện hay một hằng chuỗi.

Các ví dụ về chuỗi hợp lệ là `"string 1"` và `"\nEnter first point."`.

### Danh sách

Một danh sách `AutoLISP` là một nhóm các giá trị liên quan được tách biệt bằng các khoảng trắng và được đặt trong các dấu ngoặc đơn. Các danh sách cung cấp một phương pháp hiệu quả để lưu trữ các giá trị liên quan. `AutoCAD` biểu diễn các điểm `3D` như là một danh sách gồm ba số thực.

Ví dụ về các danh sách là `(1.0 1.0 0.0)`, `("this" "that" "the one")`, và `(1 "ONE")`.

### Tập hợp chọn

Các tập hợp chọn là các nhóm gồm một hoặc nhiều đối tượng (thực thể). Bạn có thể thêm các đối tượng vào, hoặc loại bỏ các đối tượng ra khỏi các tập hợp chọn bằng các chương trình `AutoLISP`.

Ví dụ sau đây sử dụng hàm `ssget` để cho ra một tập hợp chọn chứa tất cả các đối tượng trong một bản vẽ.

```
_$(ssget "X")
<Selection set: 1>
```

### Các tên thực thể

Một tên thực thể là một nhãn số được gán vào các đối tượng trong một bản vẽ. Nó thật sự là một `pointer` hướng sang một file được bảo quản bởi `AutoCAD` và có thể được sử dụng để tìm `record` cơ sở dữ liệu của đối tượng và các `vector` của nó (nếu chúng được hiển thị). Nhãn này có thể được tham chiếu bởi các hàm `AutoLISP` để cho phép chọn các đối tượng để xử lé thêm nhiều cách khác nhau. `AutoCAD` xem các đối tượng là các thực thể. Ví dụ sau đây sử dụng hàm `entlast` để có được tên của đối tượng sau cùng thêm vào bản vẽ.

```
_$(entlast)
<Selection set: 1>
```

### Các đối tượng VLA

Các đối tượng cho một bản vẽ có thể được biểu diễn dưới dạng các đối tượng `Visual LISP`, `ActiveX (VLA)`, một loại dữ liệu giới thiệu với `Visual LISP`. Khi làm việc với các hàm `ActiveX`, bạn phải tham chiếu các đối tượng `VLA` chứ không phải `ename pointer` được cho ra bởi các hàm chẳng hạn như `entlast`.

### Bộ mô tả file

Một bộ mô tả file là một `pointer` hướng sang một file được mở bởi hàm `open` của `AutoLISP`. Hàm `open` cho ra `pointer` này dưới dạng một nhãn chữ gạch ngang số. Bạn cung cấp bộ mô tả file dưới dạng một đối số cho các hàm `AutoLISP` khác vốn đọc hoặc ghi sang file.
Ví dụ sau đây mở file `myinfo.dat` để đọc. Hàm `open` cho ra bộ mô tả file:

```
_$(setq file1(open "c:\\myinfo.dat" "r")) #<file "c:\\myinfo.dat">
```

Trong ví dụ này, bộ mô tả file được lưu trữ trong biến `file1`.
Các file vẫn mở cho đến khi bạn đóng chúng trong chương trình `AutoLISP`. Hàm `close` đóng một file. Mã sau đây đóng file có bộ mô tả file lưu trữ trong biến `file1`.

```
_$(close file1)
	nill
```

### Các ký hiệu và biến

`AutoLISP` sử dụng các ký hiệu để tham chiếu dữ liệu. Các tên ký hiệu không nhạy kiểu chữ (tức là không phân biệt chữ HOA và chữ thường) có thể gồm bất kỳ chuỗi ký tự chữ - số và ký tự ký hiệu ngoại trừ:
*Các ký tự giới hạn từ các tên ký hiệu*
`(`			(Dấu ngoặc đơn mở)
`)`			(Dấu ngoặc đơn đóng)
`.`			(Dấu chấm)
`'`			(Apostrophe)
`"`			(Dấu trích dẫn)
`;`			(Dấu chấm phẩy)
Một tên ký hiệu không thể gồm chỉ các ký tự số.
Về mặt kỹ thuật, các trình ứng dụng `AutoLISP` hoặc các giá trị không đổi, chẳng hạn như các chuỗi, số thực và số nguyên. Từ ký hiệu `symbol` ám chỉ một tên ký hiệu vốn lưu trữ dữ liệu tĩnh, chẳng hạn như các hàm do người dùng ấn định. Từ biến `(variable)` được sử dụng để ám chỉ một tên ký hiệu vốn lưu trữ dữ liệu chương trình. Ví dụ sau đây sử dụng hàm `setq` để gán giá trị chuỗi `"this is a string"` vào biến `strl`.

```
_$(setq str1 "this is a string")
	"this is a string"
```

**Lời khuyên:** Hãy chọn các tên có ý nghĩa cho các ký hiệu và các biến chương trình của bạn.

### Các ký hiệu được bảo vệ

Bạn có thể được cảnh báo nếu bạn cố gắng thay đổi giá trị của một số ký hiệu được sử dụng bởi ngôn ngữ `AutoLISP`. Những ký hiệu này được gọi là các ký hiệu được bảo vệ và gồm có các hạng mục chẳng hạn như toán tử số học (ví dụ `+`) và các giá trị `T` và `nill`. Bạn có thể sử dụng tính năng `Visual LISP Symbol Service` để xác định xem một ký hiệu có được bảo vệ hay không.

Khi bạn khởi động `AutoCAD` trong lần đầu tiên, các ký hiệu được bảo vệ không nhận được chế độ bảo vệ đặc biệt. Nếu bạn xác lập một ký hiệu được bảo vệ tại dòng nhắc `AutoCAD Command`, bạn không được chỉ thị rằng ký hiệu có bất kỳ trạng thái đặc biệt nào. Tuy nhiên, một khi bạn khởi động `Visual LISP`, điều này thay đổi. Từ thời điểm bạn khởi động `Visual LISP` cho đến khi kết thúc tác vụ `AutoCAD` của bạn, `AutoLISP` chặn bất kỳ những nỗ lực nhằm chỉnh sửa một ký hiệu nào được bảo vệ. Việc xử lý các ký hiệu được bảo vệ sẽ phụ thuộc vào trạng thái của một số tùy chọn môi trường `Visual LISP`. Bạn có thể quyết định một trong những tùy chọn:

* **Transparent** Các ký hiệu được bảo vệ được xử lý giống như bất kỳ ký hiệu khác.
* **Print message** `AutoLISP` cấp phát một cảnh báo khi bạn chỉnh sửa bất kỳ một ký hiệu bảo vệ nhưng tiến hành sự chỉnh sửa. Ví dụ sau đây minh họa những gì xảy ra khi bạn chỉnh sửa ký hiệu `T`:
	
	```
	_$(setq t "look out"); ; User warning: assignment to protected symbol: T <- "look up" "look up"
	```
* **Prompt to enter break loop** Đây là tùy chọn mặc định, làm cho `AutoLISP` hiển thị hộp thông báo sau đây khi bạn cố gắng chỉnh sửa một ký hiệu được bảo vệ:
Nếu bạn chọn `No`, giá trị của ký hiệu được chỉnh sửa, và việc xử lý tiếp tục một cách bình thường. Nếu bạn chọn `Yes`, việc xử lý ngắt và bạn nhập vào một vòng lặp ngắt `Visual LISP`. Sự điều khiển được điều chỉnh sang cửa sổ `Visual LISP Console`. Để xác lập ký hiệu và tiếp tục xử lý, nhấn nút `Continue` trên thanh công cụ `Visual LISP`; để hủy bỏ sự chỉnh sửa, nhấn `Reset`.

* **Error** Tùy chọn này ngăn cản việc chỉnh sửa các ký hiệu được bảo vệ. Bất kỳ nỗ lực nhằm chỉnh sửa một ký hiệu được bảo vệ sẽ hiển thị một thông báo lỗi.

Để xác định cách `AutoLISP` phản ứng lại những cố gắng nhằm chỉnh sửa các ký hiệu, hãy chọn `Tool` > `Environment Option` > `General Options` từ menu `Visual LISP`. 

