---
layout: post
title:  "ObjectARX - Các yêu cầu hệ thống"
desc: "Để bắt đầu lập trình AutoCAD với ObjectARX thì máy tính của bạn phải chuẩn bị một số thứ."
keywords: "objectarx,enesy"
date: 2016-10-01
categories: [ObjectARX]
tags: [objectarx]
icon: fa-bookmark-o
---

> Bài viết này nằm trong loạt bài hướng dẫn cơ bản về ObjectARX, bài này mình dịch từ Blog của Fernando Malard tại địa chỉ: [arxdummies.blogspot.com.br](http://arxdummies.blogspot.com.br).

Trước khi bắt đầu loạt bài hướng dẫn cơ bản về ObjectARX, mình sẽ liệt kê ra đây danh sách những gì mà các bạn phải có để có thể tạo, biên dịch và sử dụng được các ứng dụng ObjectARX.

Phiên bản mình sẽ sử dụng trong suốt loạt bài này là ObjectARX 2007[^1]. I will use the ObjectARX 2005. Most of features presented will be compatible to previous versions (I will consider release 2000 and above). Future versions will certainly present more features and class/methods will slightly change. Make sure you check the latest versions at the "What's New" section of ObjectARX User's Guide.
What you will need for AutoCAD and their verticals:

* AutoCAD 2000,2000i and 2002: ObjectARX 2000 and VC++ 6.0;
* AutoCAD 2004, 2005 and 2006: ObjectARX 2004 and VC.NET 2002 (7.0);
* AutoCAD 2007, 2008 and 2009: ObjectARX 2007 and VC.NET 2005 (8.0);
* AutoCAD 2010, 2011 and 2012: ObjectARX 2010 and VC.NET 2008 (9.0);
* AutoCAD 2013 and 2014: ObjectARX 2013 and VC.NET 2010 (10.0);
* AutoCAD 2015 and 2016: ObjectARX 2015 and VC.NET 2012 (11.0);
* AutoCAD 2017: ObjectARX 2017 and VC.NET 2015 (14.0);

> Visual Studio 2010 and above provide the Platform Toolset capabilities allowing you to use their IDE but compile the code with previous version. Note that this is possible only if you have the corresponding platform core compiler version installed at your machine. The advantage of this feature is to be able to centralize multiple project versions into the same IDE.

You may download AutoCAD trial versions from Autodesk web site [AutoDesk](www.autodesk.com).
To download ObjectARX (for free), go to [ObjectARX](www.objectarx.com) (this will redirect you to the right page at Autodesk's web site).
Once you have the above products, proceed with the installation: 

* Install AutoCAD (full option recommended);
* Install Visual C++;
* Install (just need to extract) ObjectARX to your computer;
On the next post I will explain how to install the ObjectARX Wizard tool.

Further information about Microsoft Visual Studio versions:

| Product name | Codename | Internal version | cl.exe version | Supported .NET Framework versions | Release date|
|:-------------|:--------:|:----------------:|:--------------:|:---------------------------------:|------------:|
|Visual Studio |  N/A     |      4.0         |          0.0   |                 N/A          	  |  April 1995 | 
|----
|Visual Studio 97 |  Boston    |      5.0         |          0.0   |                 N/A         	  |  February 1997 | 
|----
|Visual Studio 6.0 |  Aspen    |      6.0         |          12.00   |                 N/A        	  |  June 1998 | 
|----
|Visual Studio .NET (2002) |  Rainier    |      7.0         |          13.00   |                 1.0        	  |  February 13, 2002 | 
|----
|Visual Studio .NET 2003 |  Everett    |      7.1         |          13.10   |                 1.1       	  |  April 24, 2003 | 
|----
|Visual Studio 2005 |  Whidbey    |     8.0         |         14.00   |                 2.0, 3.0       	  |  November 7, 2005 | 
|----
|Visual Studio 2008|  Orcas    |     9.0        |         15.00   |                 2.0, 3.0, 3.5       	  |  November 19, 2007 | 
|----
|Visual Studio 2010|  Dev10/Rosario    |     10.0        |         16.00   |                2.0, 3.0, 3.5, 4.0       	  |  April 12, 2010 | 
|----
|Visual Studio 2012|  Dev11    |    11.0        |         17.00   |                2.0, 3.0, 3.5, 4.0, 4.5, 4.5.1, 4.5.2       	  |  September 12, 2012 | 
|----
|Visual Studio 2013|  Dev12    |    12.0        |         18.00   |                2.0, 3.0, 3.5, 4.0, 4.5, 4.5.1, 4.5.2       	  |  October 17, 2013 | 
|----
|Visual Studio 2015|  Dev14    |    14.0        |         19.00   |                2.0, 3.0, 3.5, 4.0, 4.5, 4.5.1, 4.5.2, 4.6, 5.0       	  |  July 20, 2015 | 
|--------|
{: rules="groups"}

[^1]: Trong bài gốc Fernando Malard sử dụng ObjectARX 2005, tuy nhiên ở Việt Nam phiên bản AutoCAD phổ biến nhất là AutoCAD 2007 nên mình sẽ sử dụng ObjectARX 2007 để hướng dẫn.