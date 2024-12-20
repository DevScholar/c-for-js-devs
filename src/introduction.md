# Introduction

This is a (non-comprehensive) guide for JavaScript developers that are completely new to the C programming language. Some concepts and constructs translate fairly well between JavaScript and C, but which may be expressed differently, whereas others are a radical departure, like memory management. This guide provides a brief comparison and mapping of those constructs and concepts with concise examples.

The original authors[^authors] of this guide were themselves JavaScript developers who were completely new to C. <!--This guide is the compilation of the knowledge acquired by the authors writing Rust code over the course of several months. -->It is the guide the authors wish they had when they started on their C journey. That said, the authors would encourage you to read books and other material available on the Web to embrace C and its idioms rather than attempting to learn it exclusively through the lens of JavaScript. Meanwhile, this guide can help answers some question quickly, like: _Does C support inheritance, threading, asynchronous programming, etc.?_
Assumptions:

- Reader is a seasoned JavaScript developer.
- Reader is completely new to C.

Goals:

- Provide a brief comparison and mapping of various JavaScript topics to their counterparts in C.
- Provide links to C reference, book and articles for further reading on topics.

Non-goals:

- Discussion of design patterns and architectures.
- Tutorial on the C language.
- Reader is proficient in C after reading this guide.
- While there are short examples that contrast JavaScript and C code for some topics, this guide is not meant to be a cookbook of coding recipes in the two languages.

---
[^authors]: The original authors of Microsoft's Rust for C#/.NET Developers were (in alphabetical order):
[Atif Aziz], [Bastian Burger], [Daniele Antonio Maggio], [Dariusz Parys] and
[Patrick Schuler].

  [Atif Aziz]: https://github.com/atifaziz
  [Bastian Burger]: https://github.com/bastbu
  [Daniele Antonio Maggio]: https://github.com/danigian
  [Dariusz Parys]: https://github.com/dariuszparys
  [Patrick Schuler]: https://github.com/p-schuler

The adaption work is done by @[DevScholar] on GitHub.

[DevScholar]: https://github.com/DevScholar

This book contains artificial intelligence generated content, and the content is audited.