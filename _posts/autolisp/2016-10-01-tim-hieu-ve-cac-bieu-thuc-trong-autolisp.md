---
layout: post
title:  "Tìm hiểu về các biểu thức trong AutoLISP"
desc: "Một chương trình AutoLiSP gồm một chuỗi biểu thức. Các biểu thức AutoLISP có dạng sau đây:"
keywords: "c++,enesy"
date: 2016-10-01
categories: [AutoLISP]
tags: [autolisp]
icon: fa-file-code-o
---


### Biểu thức trong AutoLISP
Khi bạn nhập `text` tại dòng lệnh, `AutoCAD` sẽ dịch `text` này bằng cách so sánh các ký tự với danh sách các tên lệnh. Nếu `text` tương ứng với các lệnh của `AutoCAD` thì nó sẽ đánh giá và thi hành lệnh tương ứng. Khi `AutoCAD` load các `AutoLISP code` thì chúng chuyển các `code` này đến bộ biên dịch `AutoLISP`. Cấu trúc dữ liệu cơ bản của `AutoLISP` là các danh sách `(List)`. Danh sách là tập hợp các phần tử chứa trong cặp dấu ngoặc đơn `()`. Các phần tử tron danh sách phân cách nhau bởi các khoảng trắng. Có hai loại danh sách: biểu thức `(expression)` và danh sách dữ liệu `(data list)`. Biểu thức là thành phần cơ bản trong tất cả các chương trình `AutoLISP`. Phần tử đầu tiên của biểu thức là một hàm `(function)`. Hàm này sẽ được `AutoLISP` định giá trị và trả về kết quả. Các phần tử tiếp theo là tham số, cũng là các giá trị cung cấp cho hàm. Giá trị trả về là kết quả tính toán của hàm. Sự khác nhau cơ bản giữa một biểu thức `AutoLISP` với một biểu thức toán học là các phần tử `AutoLISP` có thứ tự khác với đẳng thức và chúng phải được chứa trong cặp dấu ngoặc đơn.
Một chương trình `AutoLiSP` gồm một chuỗi biểu thức. Các biểu thức `AutoLISP` có dạng sau đây:

```
(function argurments)
```

Mỗi biểu thức bắt đầu với một dấu ngoặc đơn mở (bên trái) và gồm một tên hàm và các đối số tùy ý của hàm đó. Mỗi đối số cũng có thể là một biểu thức (có nghĩa là biểu thức có thể lồng trong biểu thức). Biểu thức kết thúc bằng dấu ngoặc đơn phải. Biểu thức sẽ cho ra một kết quả và kết quả này sẽ được sử dụng cho các biểu thức xung quanh (bao quanh) nó. Giá trị của biểu thức thông dịch sau cùng sẽ được trả về biểu thức gọi.
Ví dụ, mã sau đây có ba hàm:

```
(func1 (func2( arguments) (func3 argument)))
```

Nếu bạn nhập mã này tại dòng nhắc `Visual LISP console` hoặc dòng nhắc lệnh của `AutoCAD`, bộ thông dịch `AutoLISP AutoCAD` sẽ xử lý mã này. Hàm đầu tiên, `func1`, có hai đối số, và các hàm còn lại `func2`, `func3` mỗi hàm có một đối số. Các hàm `func2` và `func3` được bao quanh bởi hàm `func1`, do đó các giá trị cho ra của chúng được truyền đến `func1` dưới dạng các đối số. Hàm `func1` lượng giá hai đối số và đưa giá trị trở về cửa sổ mà bạn đã nhập mã từ đó.
Ví dụ sau đây minh họa việc sử dụng hàm `*` (phép nhân), hàm này chấp nhận một hoặc nhiều số dưới dạng các đối số:

```
_$(* 2 27)
54
```

Bởi vì ví dụ mã này không có biểu thức xung quanh, `AutoLISP` đưa kết quả trở về cửa sổ mà bạn đã nhập mã từ đó.
Các biểu thức được xếp lồng trong các biểu thức khác trả kết quả của chúng trở về biểu thức xung quanh. Ví dụ sau đây sử dụng kết quả từ hàm `+` (phép cộng) dưới dạng một trong các đối số từ hàm `*` (phép nhân):

```
_$(* 2 (+ 5 10) 10
```

Nếu bạn nhập số dấu ngoặc đơn đóng (phải) không chính xác, `AutoLISP` hiển thị dòng nhắc sau đây:

```
(_>
```

Số dấu ngoặc đơn mở trong dòng nhắc này chỉ định bao nhiêu cấp độ của các dấu ngoặc đơn mở vẫn mở. Nếu dòng này xuất hiện, bạn phải nhập số dấu ngoặc đơn đóng được yêu cầu cho biểu thức cần được lượng giá.

```
_$(* 2 (+ 5 10
((_>))30

```

> Mẹo: Nếu bạn quen sử dụng `VisualLISP` để soạn thảo, hãy tạo thói quen đóng ngoặc ngay khi mở ngoặc, sau đó mới đưa con trỏ vào trong dấu ngoặc để viết các biểu thức. Ngoài ra, các trình soạn thảo code nổi tiếng hiện nay như: `Sublime Text`, `VIM`, ... đều hỗ trợ tự động đóng ngoặc ngay khi bạn mở ngoặc.

Một sai sót nữa thường hay mắc phải là bỏ qua các dấu trích dẫn đóng `(")` trong một chuỗi `text`, trong trường hợp này các dấu ngoặc đơn đóng được hiểu là một phần của chuỗi và không có tác dụng trong việc phân tích các dấu ngoặc đơn mở. Để xử lý tình trạng này nhấn `SHIFT + ESC` để hủy hàm, sau đó nhập lại nó một cách chính xác.

### Cú pháp hàm AutoLISP
Các quy ước sau đây mô tả cú pháp cho các hàm `AutoLISP`:

<figure class="one">
	<a href="/static/img/blog/autolisp/autolisp1.jpg"><img src="/static/img/blog/autolisp/autolisp1.jpg" alt=""></a>
</figure>

Trong ví dụ này, hàm `foo` có một đối số được yêu cầu, `string`, và một đối số tùy ý, `number`. Các đối số `number` bổ sung có thể được cung cấp. Thông thường, tên của đối số được chỉ định loại dữ liệu được mong đợi. Các ví dụ trong bảng sau đây trình bày các lệnh gọi hợp lệ và không hợp lệ đến hàm `foo`.

**Các ví dụ về lệnh gọi hàm hợp lệ và không hợp lệ**

| **Các lệnh gọi hợp lệ**  | **Các lệnh gọi không hợp lệ** |
|:-------------|---------:|
| `(foo "catch")` |  `(foo 44 13)`    |  
|----
| `(foo "catch" 22)` |  `(foo "fi" "foe" 44 13)`    |
|----
| `(foo "catch" 22 31)` |  `(foo)`    |
|----
{: rules="groups"}


## Luyện tập
**Các lệnh gọi hàm sau đây là hợp lệ hay không hợp lệ?**

1. `(foo "bar")`
2. (Quân thiết kế thêm bài tập ở đây)
3. ...
4. ...
5. ...
6. ...
7. ...

> Các bạn hãy tham gia luyện tập bằng cách comment câu trả lời của mình ở phía dưới nhé! Chúc các bạn học tốt!

