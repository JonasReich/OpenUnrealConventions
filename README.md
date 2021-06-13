# Open Unreal Conventions

The Open Unreal Conventions are an open source set of coding and asset conventions for Unreal Engine 4 projects created and maintained by Jonas Reich.

These conventions should serve as a foundation for all aspects of Unreal Engine development. This means they cover not only C++ code, but also Blueprints, Python and Asset creation. Over time we might also add some conventions for complementary tools like batch scripts for building Unreal Engine games.

## Subpages
### [Project Setup](./ProjectSetup.md)
### [Asset Conventions](./UnrealAssets/README.md)
### [Blueprint Conventions](./Blueprint/README.md)
### [C++ Coding Conventions](./C++/README.md)
### [Python Coding Conventions](./Python.md)

## WIP Status

The coding conventions are a work-in-progress project. This list shows the current status of the sub-conventions:

| domain        | progress |
|---------------|----------|
| Project Setup | 50%      |
| Assets        | 90%      |
| Blueprint     | 50%      |
| C++           | 90%      |
| Python        | 100%     |

## General Structural Rules

### Structure Content with the Uninformed Developer in Mind

You probably are not the only developer in a project. And even if you are you will most certainly come back to a piece of code, a blueprint or an asset folder that you haven't seen in a while. It is very likely that you will forget and re-learn half of the things you worked on over the course of a project.

Creating a structure that is easily parse-able for an uninformed viewer is crucial for project maintainability. And all other rules are based on this very thought.

This should usually manifest in thinking about the **big picture first** and structuring your content **from big to small**.

### The Magical Number Seven (Plus or Minus Two)

There is a [rule in psychology](https://en.wikipedia.org/wiki/The_Magical_Number_Seven,_Plus_or_Minus_Two) stating humans cannot easily separate and memorize groups of more than seven objects (plus minus seven).
We can apply this rule to our C++ code and Blueprints saying you should introduce logical groupings once you have more than 7-10 variables, functions, etc.

Of course this all depends on the context, but it's a good rule of thumb to recognize code, blueprints, etc that are too crowded.

## General Programming Rules

For all logical programming no matter the language (Blueprints, C++, Python) you must always adhere to the following basic rules:

1. Leave a file at least as clean as you found it
2. Fix errors when you stumble across them 
3. Don't submit any broken code including code that you know throws errors or warnings
4. Your code must always be clean, well formatted and properly documented
5. You must always stick to [SOLID](https://en.wikipedia.org/wiki/SOLID) principles and write [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) code
6. Consistency is king. Stick to existing conventions and establish new ones as soon as you do something not covered by conventions.
7. Never leave any dead/unreachable/commented-out code

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

## References

These coding conventions are based on a mixture of the following coding conventions:
- C++ Coding Conventions
    - [Epic Games Coding Standard](https://docs.unrealengine.com/en-US/ProductionPipelines/DevelopmentSetup/CodingStandard/index.html)
    - [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html)
    - [C++ Core Guidelines](http://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)
    - Aesir Interactive Unreal C++ Coding Conventions (not public)
- Blueprint Coding Conventions
    - [Gamemakin UE4 Style Guide](https://github.com/Allar/ue4-style-guide) aka "Allar style guide"

## Further Reading

The following sources are reccommended reading material for their respective topic:

- General
    - [Unreal Engine 4 Documentation](https://docs.unrealengine.com/en-US/index.html) - The official docs should be your first address for everything Unreal related. It used to be quite outdated with gaping holes, but since UE4.24 onwards the documentation has caught up with some of the latest developments of the Engine itself.

        Still not everything is documented in here, but for everything that is, it's usually the best introduction into each respective topic if not the most comprehensive.

    - Unreal Engine 4 Source Code - The engine's source code is shipped with every UE4 installation, so whenever you want to know what's really going on under the hood, see examples how a certain system/function should be used, etc. this is the place to look.

        The source code is really well documented (at least in the important places - some plugins could improve) and will often times contain better comments + explanations besides the code itself than the online documentation.

- Multiplayer / Networking
    - [Network Compendium by Cedric 'eXi' Neukrichen](http://cedric-neukirchen.net/Downloads/Compendium/UE4_Network_Compendium_by_Cedric_eXi_Neukirchen.pdf) - Good overview of everything network related. Explains both the high level networking architecture but also shows some low level code snippets that will cover 80% of your net-code needs.

- Gameplay Ability System
    - [GASDocumentation by tranek](https://github.com/tranek/GASDocumentation) - Very comprehensive breakdown of gameplay ability system with many practical examples, code snippets and reccommendations for different use cases (including performance optimization opportunities).

## License

The Open Unreal Conventions are licensed under the [MIT license](LICENSE.md).

## Contributing

You are invited to create [pull-requests](https://github.com/JonasReich/OpenUnrealConventions/pulls) to the github source for any additions or modifications you make to the guidelines.
