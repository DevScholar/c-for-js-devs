# Documentation Comments

A third-party tool called JSDoc provides a mechanism to document the API for types using a comment syntax. JSDoc includes a Markdown plugin that automatically converts Markdown-formatted text to HTML. The comment contains structured data representing the comments and the API signatures. Other tools can process that output to provide human-readable documentation in a different form. A simple example in JavaScript:

```js
public class MyClass {}
/**
 * This is a document comment for `MyClass`.
 * @class
 */
class MyClass {}
```

C lacks a standardized way to generate documentation from comments like JSDoc, developers can adopt tools like Doxygen to extract structured comments and generate documentation from C code. Doxygen interprets specially formatted comments to produce documentation.Doxygen serves as the tool for generating documentation. In C, Doxygen uses a specific syntax to create documentation comments. For instance, in C using Doxygen:

```c
/* 
 * This is a comment for the MyStruct struct.
 */
struct MyStruct {
    // Members of the struct
};
```

In JSDoc, the equivalent to Doxygen is `jsdoc`.

See also:

- [How to write documentation]
- [Documentation tests]

[How to write documentation]: https://www.doxygen.nl/manual/docblocks.html
