# Open Unreal Coding Conventions

These coding conventions should serve as a foundation for all aspects of Unreal Engine programming. This means they cover not only C++ code, but also Blueprints and Python.

## Subpages
- ### [Asset Conventions](./UnrealAssets/README.md)
- ### [Blueprint Conventions](./Blueprint/README.md)
- ### [C++ Coding Conventions](./C++/README.md)
- ### [Python Coding Conventions](./Python/README.md)

## WIP Status

The coding conventions are a work-in-progress project. This list shows the current status of the sub-conventions:

| domain    | progress |
|-----------|----------|
| C++       | 90%      |
| Blueprint | 5%       |
| Python    | 0%       |

## General Naming Rules

**Always use PascalCase.** This applies for assets, folder names, variable names, function names, you name it.

- Already used by Engine code
- Already used by sample content
- Only exception: [Python](Python/Naming.md)

**Never use spaces for names.** This is mainly important for asset and folder names (including the folders in which you store your unreal projet), but can also be extended to variables and functions in your Blueprints.

**Stick to Latin Alphanumerics and underscores.** Never use special unicode characters, umlauts or any other special characters. In short, stick to this set of characters:

```
ABCDEFGHIJKLMNOPQRSTUVXYZabcdefghijklmnopqrstuvxyz0123456789_
```

**Keep names short and concise.** Short _and_ readable is the key here. Other devs still need to understand what you mean. Unreal enforces a 180 character path limit for all source files and assets, so keeping file and folder names short is essential. It also makes file lists, debug logs, etc easier to read. This last point also applies to function and variable names.

## Sources

These coding conventions are based on a mixture of the following coding conventions:
- C++ Coding Conventions
    - [Epic Games Coding Standard](https://docs.unrealengine.com/en-US/ProductionPipelines/DevelopmentSetup/CodingStandard/index.html)
    - [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html)
    - [C++ Core Guidelines](http://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)
    - Aesir Interactive Unreal C++ Coding Conventions (not public)
- Blueprint Coding Conventions
    - [Gamemakin UE4 Style Guide](https://github.com/Allar/ue4-style-guide) aka "Allar style guide"
