<sub>[Home](../README.md) / C++ </sub>

# C++ Coding Conventions

<pre style="color:yellow; background: #222222eb;">NOTE: These pages are being rewritten as 'live' source code conventions
in the form of an Unreal Engine plugin.</pre>

See [UnrealPlugin](./UnrealPlugin)

----

## [Code Organization](CodeOrganization.md)
## [Naming](Naming.md)
## [Documentation](Documentation.md)
## [Formatting](Formatting.md)

## STL / std Namespace

Do not use any of the functions and types defined in the std namespace!

For all essential functions there are unreal specific substitues including but not limited to:

| std namespace        | UE4 replacement                                   |
|----------------------|---------------------------------------------------|
| ``static_cast<T>()`` | ``StaticCast<T>()`` or ``Cast<T>()`` for UObjects |
| ``move<T>()``        | ``MoveTemp<T>()``                                 |
| ``forward<T>()``     | ``Forward<T>()``                                  |
| ``unique_ptr<T>``    | ``UniquePtr<T>``                                  |
| ``shared_ptr<T>``    | ``SharedPtr<T>``                                  |
| ``weak_ptr<T>``      | ``WeakPtr<T>``                                    |
| ``vector<T>``        | ``TArray<T>``                                     |
| ``array<T>``         | ``TArray<T, TInlineAllocator<T>>``                |
| ``map<T, U>``        | ``TMap<T, U>``                                    |
| ``tuple<T...>``      | ``TTuple<T...>``                                  |

The engine source code also contains a significant amount of type traits that substitute the std type traits.

## Preprocessor / Defines / Macros

- Use the preprocessor as little as possible
    - Prefer using global const variables over defines
    - Prefer using functions/templates over macros
- Using macros is permissible in the following cases:
    - Instead of using variadic functions (not supported by all target platform compilers), e.g. log functions
    - Sparse type declarations (e.g. log categories, type trait overloads)
    - To reduce potential of string typos (e.g. enum to string conversions)
- When using preprocessor switches, use ``#if`` instead of ``#ifdef``
- Try restricting macro definition scope to avoid name collisions
    - define in source files if possible
    - ``#undef`` as early as possible

## const correctness

Use ``const`` when possible for
- global variables
- member functions
- pointer or reference function parameters

Do _not_ use const in the following situations:
- passing parameters by value (see [Parameter Passing](#parameter-passing))
- return values:
    ```cpp
    // Bad - returning a const array
    const TArray<FString> GetSomeArray();
 
    // Fine - returning a reference to a const array
    const TArray<FString>& GetSomeArray();
 
    // Bad - returning a const pointer to a const array
    const TArray<FString>* const GetSomeArray();
    
    // Fine - returning a pointer to a const array
    const TArray<FString>* GetSomeArray();
    ```

## Parameter Passing

Source: [C++ Core Guidelines F.15](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#f15-prefer-simple-and-conventional-ways-of-passing-information):
- Prefer simple to understand ways of passing paramters
- When using an "unusual and clever" way to pass a parameter, document the reason (e.g. don't add explicit support for move semantics without good reason)

Stick to the following table when possible and document decisions whenever you pick a different approach:

| parameter functionality | Cheap or impossible to copy type (e.g. ``int``, ``FUnqiuePtr<T>``) | Cheap to move (e.g. ``TArray<T>``, ``FString``) _or_ Moderate cost to move (e.g. ``TArray<TMap<T, U>>``, ``BigPOD``) or Don't know (e.g. unfamiliar type, template) | Expensive to move (e.g. ``BigPOD[]``) |
|-|-|-|-|
| Out | ``X f()`` | ``X f()`` | ``f(X&)`` |
| In/Out | ``f(X&)`` | ``f(X&)`` | ``f(X&)`` |
| In & read only | ``f(X)`` | ``f(const X&)`` | ``f(const X&)`` |
| In & retain copy | ``f(X)`` | ``f(const X&)`` | ``f(const X&)`` |

When you really want to add explicit move semantics support, check if your parameter falls into one of the following cases:

| parameter functionality | Cheap or impossible to copy type (e.g. ``int``, ``FUnqiuePtr<T>``) | Cheap to move (e.g. ``TArray<T>``, ``FString``) _or_ Moderate cost to move (e.g. ``TArray<TMap<T, U>>``, ``BigPOD``) or Don't know (e.g. unfamiliar type, template) | Expensive to move (e.g. ``BigPOD[]``) |
|-|-|-|-|
| Out | ``X f()`` | ``X f()`` | ``f(X&)`` |
| In/Out | ``f(X&)`` | ``f(X&)`` | ``f(X&)`` |
| In & read only | ``f(X)`` | ``f(const X&)`` | ``f(const X&)`` |
| In & retain copy | ``f(X)`` | ``f(const X&)`` + ``f(X&&) & move`` | ``f(const X&)`` |
| In & move from | ``f(X&&)`` | ``f(X&&)`` | ``f(X&&)`` |

## Constructor Overloads

- Declare parameterless default constructor using ``= default;`` syntax where possible
- Always declare a parameterless constructor for reflected types (enforced by UHT)
- Omit a parameterless constructor for non-reflected types if reasonable

## Modern C++ Features (C++11 and higher)

Not all features of C++11, C++14 etc are supported by all UE4 target platforms. However following features are reccommended to use:

- ``static_assert`` use for compile time assertion. Prefer over any runtime checks, especially for templates.
- ``override`` and ``final`` are strongly encouraged for overriding virtual functions
- ``nullptr`` should be used instead of C-style ``NULL`` macro in all cases.
    - Exception: ``typeof(nullptr)`` should be replaced with ``TYPE_OF_NULLPTR`` for templates to ensure compatibility with C++/CX builds (e.g. XBox One)
- ``auto`` keyword for variable types, lambda types and template return types
    - Restrict usage to a minimum where the type is either apparent from immediate context (e.g. construction/assignment) or not expressible (e.g. lambdas, some template code)
- use range based for whenever possible
    - check if iterators have range alternative (e.g. ``TActorRange`` instead of ``TActorIterator``)
- lambdas
    - lambdas should be as short as possible and replaced with named functions if they exceed a certain length
    - avoid stateful lambdas
    - use explicit capture (e.g. ``[&Foo]`` instead of automatic capture (``[&]`` and ``[=]``)
    - use trailing return type whenever possible (e.g. ``[]() -> bool {return true;}``) to make compiler output more relevant/precise
- strongly typed enums
    - use enum classes instead of plain enums wherever possible
    - only exception: enum flags used in conjunction with enum classes:
        ```cpp
        // If you do use plain enums, always wrap them with a "namespace" struct:
        struct EFlags
        {
            enum Type
            {
                Alpha = 0b001,
                Beta = 0b010,
                Gamma = 0b100
            }
        };

        enum class EActualEnum
        {
            Foo = EFlags::Alpha | EFlags::Beta,
            Bar = EFlags::Beta | EFlags::Gamma
        };
        ```
- Binary literals should be used when you care about binary representation (e.g. when using bitmasks, see above)

## Include Style

To ensure your module's header files can be included by other modules, you should stick to these two rules:

1. Never use relative paths using ``./`` or ``../`` notation
2. Is your module split in public/private folder?
    - Yes: Use paths relative to the module parent directory for module directory, e.g.
        ```cpp
        #include "MyModule/SubFolder/HeaderFile.h"
        ```
    - No: Use paths relative to the module directory itself, e.g.
        ```cpp
        #include "SubFolder/HeaderFile.h"
        ```

## Fundamental Type Aliases

Instead of using built-in types like int and short, the UE4 typedefs should be used:
- NULLPTR_TYPE for the type of nullptr, the nullptr literal should still be used
- TCHAR for character
- uint8, uint16, uint32, uint64 for unsigned integers
- int8, int16, int32, int64 for signed integers

Standard types that may be used:
- bool
- float
- double

## Literals

String and number literals may be used. Do not use raw string literals (``"foo"``, ``L"bar"``), but instead always use the ``TEXT("foo")`` macro to surround string literals, unless they are fed into other macros, such as ``INVTEXT("My culture invariant")``. 

If you need FTexts for UI display, never convert from FString, always write localizable FTexts using ``LOCTEXT()``, or culture invariants using ``INVTEXT()`` for dev UI that should not be translated:

```cpp
// localizable button label
FText ButtonLabel = LOCTEXT("ButtonLabel", "Press Me!");

// culture invariant used for DEV UI, so it doesn't end up in loca kits
FText DevButtonLabel = LOCTEXT("DevButtonLabel", "Press Me! (only if you're a dev)");
```

## Log Macros

Never use the LogTemp log category for anything other than temporary debug code. Always use/create an appropriate log cateogry using one of the declare/define log cateogry macros instead (see [UE4 Community wiki](https://unrealcommunity.wiki/logging-lgpidy6i)).
