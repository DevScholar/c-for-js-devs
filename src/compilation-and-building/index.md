# Compilation and Building

## JavaScript CLI

There is no concept of CLI in the JavaScript standard. People often use non-browser runtimes such as Node.js and Deno to act as CLIs.

There is no concept of CLI in the C standard, different IDEs and different toolchains have different CLIs.
## Building

When building JavaScript, the scripts coming from dependent packages are generally co-located with the project's output assembly. C compilers compiles the project sources, except the C compiler statically links all code into a single, platform-dependent, binary.

Developers use different ways to prepare a JavaScript executable for distribution, either as a _framework-dependent deployment_ (FDD) or _self-contained deployment_ (SCD). In Rust, there is no way to let the build output already contains a single, platform-dependent binary for each target.

In C, the build output is, again, a platform-dependent, compiled library for each library target.

## Dependencies

There is no concept of dependency in the JavaScript standard. However, some JavaScript runtimes, such as Node.js and Deno, have the concept of dependencies. In Node.js and Deno, the contents of a project file (`package.json`) define the build options and dependencies. A typical project file will look like:

```json
{
  "name": "your-project-name",
  "version": "1.0.0",
  "description": "Your project description",
  "dependencies": {
    "linq": "4.0.3"
  }
}
```

There is no concept of dependency in the C standard, different IDEs and different toolchains have different ways to manage dependencies.

## Packages

There is no concept of packages in the JavaScript standard. However, some JavaScript runtimes, such as Node.js, have the concept of packages. NPM is most commonly used to install packages for Node.js, and various tools supported it.
For example, adding a Node.js package reference with the Node,js CLI will add the
dependency to the project file:

  npm install linq

The most common package registry for Node.js is [npmjs.com] .

[npmjs.com]: https://www.npmjs.com/

There is no concept of packages in the C standard, different IDEs and different toolchains have different ways to manage packages.

## Static code analysis

ESLint is an analyzer that provide code quality as well as code-style analysis. The equivalent linting tool in C is Clang Static Analyzer or other tools.