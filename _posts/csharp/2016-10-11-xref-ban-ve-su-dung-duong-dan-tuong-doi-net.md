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