<sub>[Home](../README.md) / [Assets](./README.md) / Folder Hierarchy </sub>

# Folder Hierarchy

## Top Level Folder
Use a top-level folder for project specific assets
- to not pollute "global" namespace
- to reduce migration conflicts so assets can be easily transferred to other projects

## Developers Folder
Use Developers folder for local testing
- Makes testing content easy to differentiate from "production" content
- Allows hiding your test content for other devs

## Subfolders
Only create subfolders for asset types if you can put a significant number of assets in them. (e.g. do not create subfolders for textures/materials by default unless it's a big "library")

## Domain Root Folders
Some domain specific files belong in dedicated root folders separated by domain:
- **Art**<br>
    Keep all art assets under this folder. Allows integrating art assets and grooming the asset library easier. Group by context (e.g. characters, vehicles, items, environment, GUI)
    - **MaterialLibrary**<br>
        Contains all base materials. Makes it easy to find base materials and keep an overview. This is like the "Core" folder for TechArtists
    - **VFX**<br>
        Visual effects are also art and require a lot of custom art (custom textures etc) that would be weird to store elsewhere
- **Balancing**<br>
    Allows GD to keep an overview of all assets they need for tuning the game. This is the "Core" folder for GD
- **Core**<br>
    Only tech/techart edit content in here. Create child blueprints for GD/Art modifications
    - **Tests**<br>
        Automated testing content
    - **Editor**<br>
        Editor only content like editor utilities
- **Maps**<br>
    Keep an overview of all levels. Simplifies managing packaged content / life of build engineers, level designers, etc
- **Placeables**<br>
    Simplifies LD work because they can just drag and drop content from here into the level
- **Sounds**<br>
    Makes syncing audio files with external sources easier. They will be referenced all over the place anyways which makes it messy enough already
- **Text**<br>
    Allows GD + Writers to keep an overview of written text in the game. Only this and "Balancing" should contain text that is displayed anywhere.
