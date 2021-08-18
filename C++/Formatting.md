<sub>[Home](../README.md) / [C++](./README.md) / Formatting </sub>

# C++ Code Formatting

## Braces

Never omit braces with two exceptions:
```cpp
// early return:
if (condition)
    return;

// early continue:
if (condition)
    continue;
```

Always place braces on new lines:
```cpp
if (condition)
{
    statements;
}
else
{
    statements;
}
```

## Indentation

- Indent using tabs (4 characters)
- Align text following non-tab characters (e.g. function parameters, comments) using spaces:
    ```cpp
    // Optionally align function parameters to aid legibility
    MyFunction(FirstParam,        SecondParam, true,  "some string");
    MyFunction(FirstParamVariant, SecondParam, false, "some different string");
    ```
- Indent code by execution blocks
- Indent 2nd/3rd/.. line of statements with line-breaks, e.g.
    ```cpp
    UE_CLOG(LogTemp, Log, TEXT("My log message with many parameters %s, %i, %s"),
        *StringParameter1, // next lines of statement indented by a single tab
        IntParameter, StringParaneter2);
    // continue without indent after statement
    ```
- Use the same indentation rules for C# and Python files inside an Unreal Project

## Spaces within Statements

- Add a space between if/for and parentheses
    ```cpp
    if (myCondition) // good
    if(myCondition)  // bad
    ```
- Don't add a space between function name and parentheses
    ```cpp
    MyFunction();  // good
    MyFunction (); // bad
    ```
- Spaces in type declarations:
    - Don't add spaces between type name and `*` or `&`
    - Do add a space before and after const
    - Do add a space between type and variable name
    - Examples:
        ```cpp
        // good
        FFoo* MyFoo;
        const FFoo* MyFoo;
        FFoo* const MyFoo;
        FFoo const * const MyFoo;

        // bad
        FFoo * MyFoo;
        FFoo*MyFoo;
        FFoo*const MyFoo;
        FFoo const*const MyFoo;
        ```

## Switch Statements

- each case must end with one of the following:
    - ``return;``
    - ``break;``
    - ``// falls through``
- cases with multiple lines must be surrounded by braces
- always include a ``default`` case

## Empty lines

- Leave at most two empty lines between declarations
- Always add an empty line at the bottom of a file to avoid parsing errors with some compilers such as gcc
