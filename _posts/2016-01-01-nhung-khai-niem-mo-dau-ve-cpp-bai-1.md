---
layout: post
title:  "Những khái niệm mở đầu về C++ Bài 1"
desc: "Mục đích của chương này là cung cấp cho bạn những kiến thức ban đầu về C++."
keywords: "c++,enesy"
date: 2016-010-01
categories: [C++]
tags: [blog]
icon: fa-bookmark-o
---

## 2.1 Giới thiệu

> Đây là loạt bài mình dịch theo cuốn "The C++ Programming Language" của Bjarne Stroustrup (cha đẻ của C++). Các bạn có thể xem qua đánh giá về quyển sách này [tại đây](http://dangkhoasdc.github.io/review-the-C++-programming-language/).

The aim of this chapter and the next three is to give you an idea of what C++ is, without going into
a lot of details. This chapter informally presents the notation of C++, C++’s model of memory and
computation, and the basic mechanisms for organizing code into a program. These are the language facilities supporting the styles most often seen in C and sometimes called procedural programming. Chapter 3 follows up by presenting C++’s abstraction mechanisms. Chapter 4 and
Chapter 5 give examples of standard-library facilities.

The assumption is that you have programmed before. If not, please consider reading a textbook, such as Programming: Principles and Practice Using C++ [Stroustrup,2009], before continuing here. Even if you have programmed before, the language you used or the applications you
wrote may be very different from the style of C++ presented here. If you find this ‘‘lightning tour’’
confusing, skip to the more systematic presentation starting in Chapter 6.

This tour of C++ saves us from a strictly bottom-up presentation of language and library facilities by enabling the use of a rich set of facilities even in early chapters. For example, loops are not
discussed in detail until Chapter 10, but they will be used in obvious ways long before that. Similarly, the detailed description of classes, templates, free-store use, and the standard library are
spread over many chapters, but standard-library types, such as vector, string, complex, map,
unique_ptr, and ostream, are used freely where needed to improve code examples.

As an analogy, think of a short sightseeing tour of a city, such as Copenhagen or New York. In
just a few hours, you are given a quick peek at the major attractions, told a few background stories,
and usually given some suggestions about what to see next. You do not know the city after such a
tour. You do not understand all you have seen and heard. To really know a city, you have to liv e in
it, often for years. However, with a bit of luck, you will have gained a bit of an overview, a notion
of what is special about the city, and ideas of what might be of interest to you. After the tour, the
real exploration can begin.
This tour presents C++ as an integrated whole, rather than as a layer cake. Consequently, it
does not identify language features as present in C, part of C++98, or new in C++11. Such historical information can be found in §1.4 and Chapter 44.