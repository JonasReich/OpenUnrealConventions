# Open Unreal Coding Conventions

These coding conventions should serve as a foundation for all aspects of Unreal Engine programming. This means they cover not only C++ code, but also Blueprints and Python.

## Subpages
### [Asset Conventions](./UnrealAssets/README.md)
### [Blueprint Conventions](./Blueprint/README.md)
### [C++ Coding Conventions](./C++/README.md)
### [Python Coding Conventions](./Python/README.md)

## WIP Status

The coding conventions are a work-in-progress project. This list shows the current status of the sub-conventions:

| domain    | progress |
|-----------|----------|
| Assets    | 90%      |
| Blueprint | 50%      |
| C++       | 90%      |
| Python    | 0%       |

## General Structural Rules

### Structure Content with the Uninformed Developer in Mind

You probably are not the only developer in a project. And even if you are you will most certainly come back to a piece of code, a blueprint or an asset folder that you haven't seen in a while. It is very likely that you will forget and re-learn half of the things you worked on over the course of a project.

Creating a structure that is easily parse-able for an uninformed viewer is crucial for project maintainability. And all other rules are based on this very thought.

This should usually manifest in thinking about the **big picture first** and structuring your content **from big to small**.

### The Magical Number Seven (Plus or Minus Two)

There is a [rule in psychology](https://en.wikipedia.org/wiki/The_Magical_Number_Seven,_Plus_or_Minus_Two) stating humans cannot easily separate and memorize groups of more than seven objects (plus minus seven).
We can apply this rule to our C++ code and Blueprints saying you should introduce logical groupings once you have more than 7-10 variables, functions, etc.

Of course this all depends on the context, but it's a good rule of thumb to recognize code, blueprints, etc that are too crowded.

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
