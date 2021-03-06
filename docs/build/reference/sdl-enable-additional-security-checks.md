---
title: "/sdl (Enable Additional Security Checks)"
ms.date: "11/26/2018"
f1_keywords: ["VC.Project.VCCLCompilerTool.SDLCheck"]
ms.assetid: 3dcf86a0-3169-4240-9f29-e04a9f535826
---
# /sdl (Enable Additional Security Checks)

Adds recommended Security Development Lifecycle (SDL) checks. These checks include extra security-relevant warnings as errors, and additional secure code-generation features.

## Syntax

```
/sdl[-]
```

## Remarks

**/sdl** enables a superset of the baseline security checks provided by [/GS](gs-buffer-security-check.md) and overrides **/GS-**. By default, **/sdl** is off. **/sdl-** disables the additional security checks.

## Compile-time Checks

**/sdl** enables these warnings as errors:

|Warning enabled by /sdl|Equivalent command-line switch|Description|
|------------------------------|-------------------------------------|-----------------|
|[C4146](../../error-messages/compiler-warnings/compiler-warning-level-2-c4146.md)|/we4146|A unary minus operator was applied to an unsigned type, resulting in an unsigned result.|
|[C4308](../../error-messages/compiler-warnings/compiler-warning-level-2-c4308.md)|/we4308|A negative integral constant converted to unsigned type, resulting in a possibly meaningless result.|
|[C4532](../../error-messages/compiler-warnings/compiler-warning-level-1-c4532.md)|/we4532|Use of `continue`, `break` or `goto` keywords in a `__finally`/`finally` block has undefined behavior during abnormal termination.|
|[C4533](../../error-messages/compiler-warnings/compiler-warning-level-1-c4533.md)|/we4533|Code initializing a variable will not be executed.|
|[C4700](../../error-messages/compiler-warnings/compiler-warning-level-1-and-level-4-c4700.md)|/we4700|Use of an uninitialized local variable.|
|[C4703](../../error-messages/compiler-warnings/compiler-warning-level-4-c4703.md)|/we4703|Use of a potentially uninitialized local pointer variable.|
|[C4789](../../error-messages/compiler-warnings/compiler-warning-level-1-c4789.md)|/we4789|Buffer overrun when specific C run-time (CRT) functions are used.|
|[C4995](../../error-messages/compiler-warnings/compiler-warning-level-3-c4995.md)|/we4995|Use of a function marked with pragma [deprecated](../../preprocessor/deprecated-c-cpp.md).|
|[C4996](../../error-messages/compiler-warnings/compiler-warning-level-3-c4996.md)|/we4996|Use of a function marked as [deprecated](../../cpp/deprecated-cpp.md).|

## Runtime checks

When **/sdl** is enabled, the compiler generates code to perform these checks at run time:

- Enables the strict mode of **/GS** run-time buffer overrun detection, equivalent to compiling with `#pragma strict_gs_check(push, on)`.

- Performs limited pointer sanitization. In expressions that do not involve dereferences and in types that have no user-defined destructor, pointer references are set to a non-valid address after a call to `delete`. This helps to prevent the reuse of stale pointer references.

- Performs class member pointer initialization. Automatically initializes class members of pointer type to **nullptr** on object instantiation (before the constructor runs). This helps prevent the use of uninitialized pointers that the constructor does not explicitly initialize. The compiler-generated member pointer initialization is called as long as:

  - The object is not allocated using a custom (user defined) `operator new`

  - The object is not allocated as part of an array (for example `new A[x]`)

  - The class is not managed or imported

  - The class has a user-defined default constructor.

  To be initialized by the compiler-generated class initialization function, a member must be a pointer, and not a property or constant.

## Remarks

For more information, see [Warnings, /sdl, and improving uninitialized variable detection](https://cloudblogs.microsoft.com/microsoftsecure/2012/06/06/warnings-sdl-and-improving-uninitialized-variable-detection/).

#### To set this compiler option in the Visual Studio development environment

1. Open the project's **Property Pages** dialog box. For details, see [Set C++ compiler and build properties in Visual Studio](../working-with-project-properties.md).

1. Select the **C/C++** folder.

1. On the **General** page, select the option from the **SDL checks** drop-down list.

## See also

[MSVC Compiler Options](compiler-options.md)<br/>
[MSVC Compiler Command-Line Syntax](compiler-command-line-syntax.md)
