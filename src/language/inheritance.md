# Inheritance

As explained in [structures] section, C does not provide (class-based) inheritance. In C, there is no direct support for traits or supertraits. To achieve similar functionality in C, one can use structures and function pointers. By defining a structure that holds function pointers to shared behaviors and then implementing those behaviors in separate functions, one can simulate trait-like behavior in C. This approach allows for defining relationships between different structs by sharing common behavior through function pointers within the structs.

[structures]: ./custom-types/structs.md