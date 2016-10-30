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

### Các loại dữ liệu Trong AutoLISP

Các biểu thức `AutoLISP` được xử lý theo thứ tự và loại dữ liệu của mã trong các dấu ngoặc đơn. Trước khi bạn hoàn toàn có thể sử dụng `AutoLISP`, bạn phải hiểu những điểm khác biệt giữa các loại dữ liệu và cách sử dụng chúng.

#### Số nguyên

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

#### Số thực

Một số thực là một số chứa một dấu thập phân. Các số thực được lưu trữ theo dạng dấu động có độ chính xác kép, cung cấp tối thiểu 14 chữ số chính xác có nghĩa. Chú ý rằng `VLISP` không hiển thị cho bạn chữ số có nghĩa.

#### Chuỗi

#### Danh sách

#### Tập hợp chọn

#### Các tên thực thể


#### Các đối tượng VLA

#### Bộ mô tả file

#### Các ký hiệu và biến

#### Các ký hiệu được bảo vệ



