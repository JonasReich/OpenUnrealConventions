<sub>[Home](../README.md) / [Blueprint](./README.md) / Asset Naming </sub>

# Asset Naming

These asset naming convention is a modified version of the [Gamemakin Asset Conventions](https://github.com/Allar/ue4-style-guide/blob/master/README.md).

All asset names follow the basic syntax ``Prefix_BaseName_Variant_Counter_Suffix``:

| Part | Mandatory/Optional | Description |
|-|-|-|
| ``Prefix`` | mandatory | Type based prefix (see below) |
| ``BaseName`` | mandatory | Easily recognizable name that identifies the asset and related assets. This is usually what you would colloquially call the object (e.g. Sunflower, Bench, Rock, etc) |
| ``Variant`` | optional | Unique variants of the asset with a clear name (e.g. Diry, Clean, Broken, Glowing) |
| ``Counter`` | optional | If you have multiple assets with matching base name and variant or if you can't reasonably find variant names, you can use a two digit counter. For consistency you should always use two digits. If you have more than 99 assets of the same type you should usually create other groupings instead of continuing to increase the counter. |
| ``Suffix`` | optional _or_ mandatory | Type based suffix (see below) |

## Prefix and Suffix

Prefixes and suffixes are dependent of the asset type. Theoretically you could use no prefixes or suffixes as Unreal allows displaying and filtering the asset type without resorting to the file name, however this stops to work as soon as you're dealing with raw file paths, assets on disk, in version control, etc.

Placing this crucial information at the same positions in the file name makes it easy to get an overview at a glance as well as making filter expressions (e.g. regex) for these files easier.

### Most Common Asset Types

| Asset Type              | Prefix     | Suffix     | Notes                             |
| ----------------------- | ---------- | ---------- | --------------------------------- |
| Level / Map             |            |            | should be in a folder called Maps |
| Level (Persistent)      |            | _P         |                                   |
| Level (Audio)           |            | _Audio     |                                   |
| Level (Lighting)        |            | _Lighting  |                                   |
| Level (Geometry)        |            | _Geo       |                                   |
| Level (Gameplay)        |            | _Gameplay  |                                   |
| Blueprint               | BP_        |            |                                   |
| Material                | M_         |            |                                   |
| Static Mesh             | SM_        |            |                                   |
| Skeletal Mesh           | SK_        |            |                                   |
| Texture                 | T_         | _?         | See [Textures](###Textures)       |
| Particle System         | PS_        |            |                                   |
| Widget Blueprint        | WBP_       |            |                                   |

### Animations

| Asset Type              | Prefix     | Suffix     | Notes                            |
| ----------------------- | ---------- | ---------- | -------------------------------- |
| Aim Offset              | AO_        |            |                                  |
| Aim Offset 1D           | AO_        |            |                                  |
| Animation Blueprint     | ABP_       |            |                                  |
| Animation Composite     | AC_        |            |                                  |
| Animation Montage       | AM_        |            |                                  |
| Animation Sequence      | A_         |            |                                  |
| Blend Space             | BS_        |            |                                  |
| Blend Space 1D          | BS_        |            |                                  |
| Level Sequence          | LS_        |            |                                  |
| Morph Target            | MT_        |            |                                  |
| Paper Flipbook          | PFB_       |            |                                  |
| Rig                     | RIG_       |            |                                  |
| Skeletal Mesh           | SK_        |            |                                  |
| Skeleton                | SKEL_      |            |                                  |

### AI

| Asset Type              | Prefix     | Suffix     | Notes                            |
| ----------------------- | ---------- | ---------- | -------------------------------- |
| AI Controller           | AIC_       |            |                                  |
| Behavior Tree           | BT_        |            |                                  |
| Blackboard              | BB_        |            |                                  |
| Decorator               | BTDecorator_ |          |                                  |
| Service                 | BTService_ |            |                                  |
| Task                    | BTTask_    |            |                                  |
| Environment Query       | EQS_       |            |                                  |
| EnvQueryContext         | EQS_       | Context    |                                  |

### Blueprints

| Asset Type              | Prefix     | Suffix     | Notes                            |
| ----------------------- | ---------- | ---------- | -------------------------------- |
| Blueprint               | BP_        |            |                                  |
| Blueprint Component	  | BP_	       | Component  | I.e. BP_InventoryComponent       |
| Blueprint Function Library | BPFL_   |            |                                  |
| Blueprint Interface     | BPI_       |            |                                  |
| Blueprint Macro Library | BPML_      |            | Do not use macro libraries if possible. |
| Enumeration             | E          |            | No underscore.                   |
| Structure               | F or S     |            | No underscore.                   |
| Tutorial Blueprint      | TBP_       |            |                                  |
| Widget Blueprint        | WBP_       |            |                                  |
| Blueprint Editor Utility | BPU_      |            |                                  |
| Editor Utility Widget   | WBPU_      |            |                                  |

### Materials

| Asset Type                    | Prefix     | Suffix     | Notes                            |
| ----------------------------- | ---------- | ---------- | -------------------------------- |
| Material                      | M_         |            |                                  |
| Material (Post Process)       | PP_        |            |                                  |
| Material Function             | MF_        |            |                                  |
| Material Instance             | MI_        |            |                                  |
| Material Parameter Collection | MPC_       |            |                                  |
| Subsurface Profile            | SP_        |            |                                  |
| Physical Materials            | PM_        |            |                                  |
| Decal                         | M_, MI_    | _Decal     |                                  |

### Textures

| Asset Type              | Prefix     | Suffix     | Notes                            |
| ----------------------- | ---------- | ---------- | -------------------------------- |
| Texture                 | T_         |            |                                  |
| Texture (Diffuse/Albedo/Base Color)| T_ | _D | same suffix with or without alpha channel |
| Texture (Diffuse/Albedo/Base Color/Opacity)| T_ | _D | same suffix with or without alpha channel |
| Texture (Normal)        | T_         | _N         |                                  |
| Texture (Roughness)     | T_         | _R         |                                  |
| Texture (Alpha/Opacity) | T_         | _O         |                                  |
| Texture (Ambient Occlusion) | T_     | _AO        |                                  |
| Texture (Bump)          | T_         | _B         |                                  |
| Texture (Emissive)      | T_         | _E         |                                  |
| Texture (Mask)          | T_         | _M         |                                  |
| Texture (Specular)      | T_         | _S         |                                  |
| Texture (Metallic)      | T_         | _M         |                                  |
| Texture (Flowmap)       | T_         | _F         |                                  |
| Texture (Heightmap)     | T_         | _H         |                                  |
| Texture (Packed)        | T_         | _*         | See notes below about packing    |
| Texture Cube            | TC_        |            |                                  |
| Media Texture           | MT_        |            |                                  |
| Render Target           | RT_        |            |                                  |
| Cube Render Target      | RTC_       |            |                                  |
| Texture Light Profile   | TLP        |            |                                  |

If textures are packed together the individual color channels should be represented by the suffix, e.g.

| Red Channel | Green Channel | Blue Channel      | Texture Suffix |
| ----------- | ------------- | ----------------- | -------------- |
| Roughness   | Metallic      | Ambient Occlusion | _RMA           |
| Opacity     | Emissive      | _empty_           | _AE            |

### Miscellaneous

| Asset Type                 | Prefix     | Suffix     | Notes                            |
| -------------------------- | ---------- | ---------- | -------------------------------- |
| Animated Vector Field      | VFA_       |            |                                  |
| Camera Anim                | CA_        |            |                                  |
| Color Curve                | Curve_     | _Color     |                                  |
| Curve Table                | Curve_     | _Table     |                                  |
| Data Asset                 | DA_        |            |                                  |
| Data Table                 | DT_        |            |                                  |
| Float Curve                | Curve_     | _Float     |                                  |
| Foliage Type               | FT_        |            |                                  |
| Force Feedback Effect      | FFE_       |            |                                  |
| Landscape Grass Type       | LG_        |            |                                  |
| Landscape Layer            | LL_        |            |                                  |
| Matinee Data               | Matinee_   |            |                                  |
| Media Player               | MP_        |            |                                  |
| Object Library             | OL_        |            |                                  |
| Sprite Sheet               | SS_        |            |                                  |
| Static Vector Field        | VF_        |            |                                  |
| Substance Graph Instance   | SGI_       |            |                                  |
| Substance Instance Factory | SIF_       |            |                                  |
| Touch Interface Setup      | TI_        |            |                                  |
| Vector Curve               | Curve_     | _Vector    |                                  |

### Paper 2D

| Asset Type              | Prefix     | Suffix     | Notes                            |
| ----------------------- | ---------- | ---------- | -------------------------------- |
| Paper Flipbook          | PFB_       |            |                                  |
| Sprite                  | SPR_       |            |                                  |
| Sprite Atlas Group      | SPRG_      |            |                                  |
| Tile Map                | TM_        |            |                                  |
| Tile Set                | TS_        |            |                                  |

### Physics

| Asset Type              | Prefix     | Suffix     | Notes                            |
| ----------------------- | ---------- | ---------- | -------------------------------- |
| Physical Material       | PM_        |            |                                  |
| Physics Asset	          | PHYS_      |            |                                  |
| Destructible Mesh       | DM_        |            |                                  |

### Sound

| Asset Type              | Prefix     | Suffix     | Notes                            |
| ----------------------- | ---------- | ---------- | -------------------------------- |
| Dialogue Voice          | DV_        |            |                                  |
| Dialogue Wave           | DW_        |            |                                  |
| Media Sound Wave        | MSW_       |            |                                  |
| Reverb Effect           | Reverb_    |            |                                  |
| Sound Attenuation       | ATT_       |            |                                  |
| Sound Class             |            |            | No prefix/suffix. Should be put in a folder called SoundClasses |
| Sound Concurrency       |            | _SC        | Should be named after a SoundClass |
| Sound Cue               | A_         | _Cue       |                                  |
| Sound Mix               | MIX_       |            |                                  |
| Sound Wave              | A_         |            |                                  |

### User Interface

| Asset Type              | Prefix     | Suffix     | Notes                            |
| ----------------------- | ---------- | ---------- | -------------------------------- |
| Font                    | Font_      |            |                                  |
| Slate Brush             | Brush_     |            |                                  |
| Slate Widget Style      | Style_     |            |                                  |
| Widget Blueprint        | WBP_       |            |                                  |
| Editor Utility Widget   | WBPU_      |            |                                  |

### VFX Effects

| Asset Type              | Prefix     | Suffix     | Notes                            |
| ----------------------- | ---------- | ---------- | -------------------------------- |
| Particle System         | PS_        |            |                                  |
| Material (Post Process) | PP_        |            |                                  |
| Niagara Emitter         | NE_        |            |                                  |
| Niagara Module          | NM_        |            |                                  |
| Niagara Function Script | NM_        | _S         |                                  | 	
| Niagara System          | NS_        |            |                                  |
