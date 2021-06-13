<sub>[Home](../README.md) / Python </sub>

# Python Coding Conventions

Our rules for Python code in Unreal Engine are fairly straight-forward: Stick to [PEP 8](https://www.python.org/dev/peps/pep-0008) and you're good.

Depending on how much Python code you write and how runtime-critical it is you may want to extend those rules for certain projects, but even then I would discourage coming up with your own rules and instead use the [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html) instead. It's more verbose than PEP 8 and contains more guidelines on how to write and document code. 

I don't want to copy-paste big parts of either those conventions here and reccommend you pick either one of them and stick with it. Nevertheless, here is a bullet point list of a few rules that are often ignored by newcomers to Python, especially in an Unreal context where they are sometimes at odds with the C++ style guide.

- Use 4 spaces for indentation (with exceptions for hanging indents / continuation lines)
- Encode files in UTF-8
- Refer to this table for naming (copied from the [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html#3164-guidelines-derived-from-guidos-recommendations) based on [Guido](https://en.wikipedia.org/wiki/Guido_van_Rossum)'s Recommendations)

    | Type                       | Public             | Internal                        |
    |----------------------------|--------------------|---------------------------------|
    | Packages                   | lower_with_under   |                                 |
    | Modules                    | lower_with_under   | _lower_with_under               |
    | Classes                    | CapWords           | _CapWords                       |
    | Exceptions                 | CapWords           |                                 |
    | Functions                  | lower_with_under() | _lower_with_under()             |
    | Global/Class Constants     | CAPS_WITH_UNDER    | _CAPS_WITH_UNDER                |
    | Global/Class Variables     | lower_with_under   | _lower_with_under               |
    | Instance Variables         | lower_with_under   | _lower_with_under (protected)   |
    | Method Names               | lower_with_under() | _lower_with_under() (protected) |
    | Function/Method Parameters | lower_with_under   |                                 |
    | Local Variables            | lower_with_under   |                                 |
    
    I know, these rules conflict with the typical UE4 naming conventions, but this is Python code after all. UE4 exports types in CapWords and functions as lower_with_under too.
- File names should stick to the naming rules of Modules suffixed with .py file extension, e.g. ``my_module.py``
- Use a main function instead of writing script code into the main file scope. This makes files cleaner and makes it possible to import them into other files.
- Don't forget docs!
- UE4 specific: Use unreal.log(), etc in favor of Python's native logging functions
- Use the following sorting for files:
    1. Module Docstring
    2. From future imports
    3. Module level "dunders" (e.g. __all__, __author__, etc)
    4. Imports sorted like this:
        1. Standard library imports.
        2. Related third party imports.
        3. Local application/library specific imports.
    5. Classes and functions (no preferred order)
    6. Main function
    7. Module execution code
        
        Should be an equivalent of the following in most cases:
        ```python
        if __name__ == '__main__':
            main()
        ```
