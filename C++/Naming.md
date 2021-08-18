<sub>[Home](../README.md) / [C++](./README.md) / Naming </sub>

# C++ Naming Conventions

## File Names

- All files have the name of the namespace or main type without type prefix (e.g. ``Actor.h`` for AActor)
- All cpp files must have a header file with the same name and path relative to the module directory or public/private folder
    - Only exception: Test files and automation specs
- All tests are contained in a cpp file ending in Tests, e.g. ``ActorTests.cpp``
    - Tests may have a header file to declare types that are only required for the tests in the cpp file, but this should be rarely needed and avoided
- Automation specs are defined in files that use the name of the type that is used as a base and end in ``.spec.cpp``, e.g. ``Actor.spec.cpp``

## Capitalization

- All names of all types, fields and functions use PascalCase with a single exceptions: Loop counters i, j, k
- Macros use UPPER_CASE
- Abbreviations are usually written in pascal case: Http, Hdmi, Usb
- Abbreviations for module names to avoid naming conflicts may use upper case prefix, e.g. 
    ```cpp
    // U prefix      = UObject subclass (see below)
    // + OUU prefix  = Scope: Open Unreal Utilities
    // + CoreLibrary = Name of the class within the scope
    class UOUUCoreLibrary : public UBlueprintFunctionLibrary {}
    ```

## Type Prefixes and Suffixes

- Abstract base classes may be suffixed with Base
- Unreal Header Tool enforces the following prefixes for reflected types:

    | Prefix | Type                                                   |
    |--------|--------------------------------------------------------|
    | T      | Template classes                                       |
    | U      | UObject child classes                                  |
    | A      | AActor child classes                                   |
    | S      | SWidget child classes                                  |
    | E      | Enums (only typenames, enum items have no prefix!)     |
    | I      | UInterface classes                                     |
    | F      | Structs and all classes not covered by the rules above |

- In addition to this the following prefix convention can be found in the engine:

    | Prefix | Type                                                   |
    |--------|--------------------------------------------------------|
    | C      | Concepts for TModels trait                             |

- Blueprint function libraries are suffixed with Library, e.g. ``UMyPluginLibrary``, ``URegexLibrary``, etc.

## Template Parameters

- Type template parameters are suffixed with ``Type``. Unless you have a single unambiguous type, in which case you can use the single letter ``T``.
    ```cpp
    // good - the type names state clearly what purpose they serve
    template<typename ElementType, typename AllocatorType>
    class TArray {};

    // good - it's unambiguous that T is the target type after the cast
    template<typename T>
    T* Cast(UObject* Object);
    
    // bad - what are T and U?
    template<typename T, typename U>
    class TArray {};

    // bad - at the class header it's clear that Element is a type template parameter.
    // This information is lost in the template body, so suffixing with 'Type' is required.
    template<typename Element, typename Allocator>
    class TArray {};
    ```
- Non-type template parameters, e.g. integers have no special prefix/suffix notation, so the names are indistinguishable from static member variables, e.g. ``TInlineAllocator::NumInlineElements``
- Template template parameters (C++17 feature) are not supported by all compilers and should therefore not be used. If they do become usable in the future we're reccommending calling them TFooType, e.g.
    ```cpp
    template<typename ElementType, template<typename> typename TContainerType>
    class TMyContainerWrapper {};
    ```

## Type Aliases

Type aliases defined via typedefs or using declarations follow the regular type naming conventions with some exceptions / addendums

- Reflected types (UClasses, UStructs, UEnums) are **never** aliased
- Containers/templates referring to reflected types may only be aliased if the resulting type is not used in conjunction with uproperties
- Based on the type that is aliased, a different prefix should be picked:
    - F prefix to shorten concrete type (containers included):
        ```cpp
        // good
        using FOptionTextFilter = TTextFilter< TSharedPtr<FAvailableStringTable> >;
        using FAudioSamplePool = TMediaObjectPool<FAudioSample>;
        
        // bad
        using TOptionTextFilter = TTextFilter< TSharedPtr<FAvailableStringTable> >;
        using AudioSamplePool = TMediaObjectPool<FAudioSample>;
        ```
    - T prefix for alias templates
        ```cpp
        // good
        template <typename AttributeType>
        using TVertexAttributeIndicesArray = TAttributeIndicesArray<AttributeType, FVertexID>;
        
        // bad
        template <typename AttributeType>
        using FVertexAttributeIndicesArray = TAttributeIndicesArray<AttributeType, FVertexID>;
        template <typename AttributeType>
        using VertexAttributeIndicesArray = TAttributeIndicesArray<AttributeType, FVertexID>;
        ```
    - Type suffix to shorten template type parameters or alias types dependent on template type parameters:
        ```cpp
        template<typename ElementType, int32 NumElements>
        {
            // good
            using IteratorType = TNetworkSimBufferIterator<TNetworkSimContiguousBuffer<ElementType, NumElements>, ElementType>
           
            // bad
            using IteratorType = TNetworkSimBufferIterator<TNetworkSimContiguousBuffer<ElementType, NumElements>, ElementType>
        }
        ```
    - F or I prefix for concrete smart pointer types:
        ```cpp
        // good
        using FDataPtr = TSharedPtr<TArray<uint8>>;
        using IAssetTreeItemPtr = TSharedPtr<IAssetTreeItem>;
       
        // bad
        using TDataPtr = TSharedPtr<TArray<uint8>>;
        using AssetTreeItemPtr = TSharedPtr<IAssetTreeItem>;
        ```
    - Super to alias primary parent class (in generated headers ofc, but also sometimes for custom "F-classes"):
        ```cpp
        class Foo : public Bar, public IFooBarInterface
        {
            // good
            using Super = Bar;
            // bad - only use super for "primary" super classes
            using Super = UObject;
            // bad - not even for implemented interfaces
            using Super = IFooBarInterface;
        }
        ```
    - Type for type traits that return a type - implemented either as template or struct
        ```cpp
        // good
        template <> struct TUnsignedIntType<4> { using Type = uint32; };
        struct FHairStrandsMeshTrianglePositionFormat { using Type = FVector4; }
        
        // bad - Type alias outside of a type trait context
        using Type = TSharedPtr<TArray<uint8>>;
        ```

## Field Names

- Field names are composed of nouns.
- Boolean variables and fields must be prefixed by b.
- Boolean fields usually begin with a passive verb, often Has or Is. The rest of the name is supposed to be a noun.

```cpp
// good
bool bIsPendingDestruction;
FSkin Skin;

// bad
bool IsOutThere;          // missing prefix
bool bPendingDestruction; // missing verb
```

### Shadowing

Shadowed fields/variables are not allowed. MSVC allows variables to be shadowed from an outer scope, but GCC will not allow this, with good reason: It makes references ambiguous to a reader and hides mistakes.

For example, there are four variables with the name ``Count`` in this member function:

```cpp
int32 Count = 42;

class FSomeClass
{
public:
    void Func(const int32 Count)
    {
        for (int32 Count = 0; Count != 10; ++Count)
        {
            // Use Count
        }
    }

private:
    int32 Count = 1000;
};
```

## Functions

- Function names are verbs
- The verb describes the function's effect or return value if the function has no side effects
- Const functions that only return a boolean should be named like a question (mostly Has... or Is... prefix) which matches boolean field naming without the b prefix
- Non-const functions that do not just determine and return a value but also modify an objects state should reflect this (e.g. Check... instead of Get..., Has... or Is...)
- UFunctions interacting with Blueprints should follow some special rules:
    - BlueprintImplementableEvents should be prefixed with Blueprint
    - The native C++ equivalent of a blueprint event should be prefixed with Native
- Property replication events are called OnRep_VariableName, e.g. OnRep_HealthPoints
- Delegate bound functions begin with Handle prefix (see [Events and Delegates](#events-and-delegates))
- RPCs (remote procedure calls) are prefixed with their target, so either Server_, Client_ or Multicast_

## Function Parameters

Parameters that are passed in or in and out should be prefixed with In or InOut respectively.

```cpp
bool Trace(FHitResult& OutHitResult);
```

When calling those functions, you should use the ``OUT`` macro to mark parameters that are passed by reference and initialized by the function:

```
FHitResult HitResult;
Foo->Trace(OUT HitResult);
```

## Events and Delegates

_The following rules apply to all types of delegates in Unreal C++ (single cast delegate, multicast delegates, dynamic multicast delegates and events)._

- Delegate fields are always prefixed with On, e.g.
    ```cpp
    FComponentOverlapEvent OnComponentOverlapped;
    ```
- Delegate types that are required for a single instance may be called exactly like the instance with an F prefix:
    ```cpp
    FOnComponentOverlapped OnComponentOverlapped;
    ```
- Delegates that share the same signature may be declared with a shared delegate type ending with Delegate suffix:
    ```cpp
    FLoadDelegate;       // used for all kinds of loading events
    FGuiRequestDelegate; // used for various GUI request events
    // etc
    ```
    When in doubt, you should still create individual delegate types, esp. because it makes changing signatures easier.
- In both cases, delegate types are prefixed with F as per the usual type prefix rules
- Try to make it obvious from the delegate instance names at which time a delegate will be invoked, e.g.
    ```cpp
    // good - it's clear when the individual delegates are called
    // this would still be true if only a single one of these delegates would exist
    FLoadDelegate OnLoadRequestSubmitted;
    FLoadDelegate OnLoadStarted;
    FLoadDelegate OnLoadProgress;
    FLoadDelegate OnLoadCompleted;

    // bad - it's hard to tell when exactly this is called
    FLoadDelegate OnLoad;
    ```
- Functions that are bound to delegates should be named like the delegate instance with an additional prefix "Handle":
    ```cpp
    void HandleOnExplosion();   // good: clear 
    void OnExplosion();         // bad: OnExplosion is not a verb
    void DealExplosionDamage(); // bad: This may be called in HandleOnExplosion, but it's usually cleaner to have a separate function for delegate binding
    ```
- Delegates with parameters must document their parameters. If the usage of a parameter changes, a new delegate type should be created.

    Documentation of parameters should happen at delegate type level:
    ```cpp
    // good - add inline comment
    DECLARE_DELEGATE_OneParam(FOnFileLoaded, FString /*FilePath*/);
    
    // good - dynamic delegates force you to add parameter names anyways
    DECLARE_DYNAMIC_MULTICAST_DELEGATE_OneParam(FOnFileLoaded, FString, FilePath);
    
    // better - adding function style comment
    /**
     * Delegate that will be called when a file was loaded
     * @param FString: System file path of the newly loaded file
     */
    DECLARE_DELEGATE_OneParam(FOnFileLoaded, FString);
    ```
    Putting this info at instance level is good too, but should exactly match the type info. The basic expectation what parameters contain which information should be the same for all delegate instances!

    See this example:
    ```cpp
    //-----------------------
    // BAD CODE. DO NOT USE!
    //-----------------------

    // Delegate used for file system operations.
    DECLARE_DELEGATE_OneParam(FFileOperationDelegate, FString);
    
    /**
     * Called when a file was loaded from the file system
     * @param FString - System file path of the newly loaded file
     */
    FFileOperationDelegate OnFileLoaded;

    /**
     * Called when a file was checked out from source control
     * @param FString - Source control path relative to workspace root
     */
    FFileOperationDelegate OnFileCheckedOut;
    ```
    In the above example the contents of the string parameter changed from one delegate instance to the other, which may cause some issues if you bind functions to both delegates.
