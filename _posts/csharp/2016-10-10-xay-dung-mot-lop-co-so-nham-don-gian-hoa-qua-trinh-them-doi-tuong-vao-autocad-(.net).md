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

