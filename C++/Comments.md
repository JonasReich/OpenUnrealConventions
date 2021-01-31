<sub>[Home](../README.md) / [C++](./README.md) / Comments </sub>

# C++ Comments Conventions

## General Commenting Rules
- Comments should aid the reader and add information to the source code
- Comments that merely repeat what is already expressed in code do not serve any purpose and should be omitted
- Comments of public API should explain intent rather than explaining technical implementation
- All code must be commented reasonably including source files

## Comment Style

### API Documentation
Comments that describe API should follow the following style

- Type docs used for classes, structs, enums, namespaces:
    ```cpp
    /**
     * GameMode is a subclass of GameModeBase that behaves like a multiplayer match-based game.
     * It has default behavior for picking spawn points and match state.
     * If you want a simpler base, inherit from GameModeBase instead.
     */
    UCLASS()
    class ENGINE_API AGameMode : public AGameModeBase
    {
        ...
    };
    ```
- Function docs for global functions and member functions
    - Single line docs
        ```cpp
        /** Returns true if the match state is InProgress or other gameplay state */
        UFUNCTION(BlueprintCallable, Category="Game")
        virtual bool IsMatchInProgress() const;
        ```
    - Multi-line docs
    - Multi-line docs with parameter descriptions. Parameter descriptions may be omitted (even for individual parameters) if they add no value.
        ```cpp
        /**
         * Called from CommitMapChange before unloading previous level.
         * Used for asynchronous level streaming
         * @param PreviousMapName - Name of the previous persistent level
         * @param NextMapName - Name of the persistent level being streamed to
         */
        virtual void PreCommitMapChange(const FString& PreviousMapName, const FString NextMapName);
        ```
- Member fields should be documented in the same way as functions
- Enum cases follow the same rules, but they may also use single line comments
    - either above the variables/cases
        ```cpp
        /** Possible state of the current match, where a match is all the gameplay that happens on a single map */
        namespace MatchState
        {
            // We are entering this map, actors are not yet ticking
            extern ENGINE_API const FName EnteringMap;
            // Actors are ticking, but the match has not yet started
            extern ENGINE_API const FName WaitingToStart;
            // Normal gameplay. Specific games will have their own state machine inside this state
            extern ENGINE_API const FName InProgress;
            // Match has ended so we aren't accepting new players, but actors are still ticking
            extern ENGINE_API const FName WaitingPostMatch;
            // We are transitioning out of the map to another location
            extern ENGINE_API const FName LeavingMap;
            // Match has failed due to network issues or other problems, cannot continue
            extern ENGINE_API const FName Aborted;
        }
        ```
    - or behind the values and left aligned:
        ```cpp
        /** Possible state of the current match, where a match is all the gameplay that happens on a single map */
        namespace MatchState
        {
            extern ENGINE_API const FName EnteringMap;      // We are entering this map, actors are not yet ticking
            extern ENGINE_API const FName WaitingToStart;   // Actors are ticking, but the match has not yet started
            extern ENGINE_API const FName InProgress;       // Normal gameplay. Specific games will have their own state machine inside this state
            extern ENGINE_API const FName WaitingPostMatch; // Match has ended so we aren't accepting new players, but actors are still ticking
            extern ENGINE_API const FName LeavingMap;       // We are transitioning out of the map to another location
            extern ENGINE_API const FName Aborted;          // Match has failed due to network issues or other problems, cannot continue
        }
        ```

### Headings

Subsections, headings, etc. can use blocks of ``// single line comments`` like this:

- Headings:
    ```cpp
    //////////////////////////////////////////////////////////////////////////
    /// Big Heading: always 74 characters wide
    /// can have multiple lines
    //////////////////////////////////////////////////////////////////////////

    //------------------------------------------------------------------------
    // Medium Heading: always 74 characters wide
    // can have multiple lines
    //------------------------------------------------------------------------

    //-----------------------------
    // Small Heading + single line
    //-----------------------------
    ```
- Long separators (e.g. for separating test cases in a single test file)
    ```cpp
    //////////////////////////////////////////////////////////////////////////
    ```
    or
    ```cpp
    //------------------------------------------------------------------------
    ```
- Virtual function override groups:
    ```cpp
    // - Parent Class
    virtual void Foo() override;
    virtual void Bar() override;
    // --
    ```

### Inline comments

Comments that are placed inline in function definitions and source files should usually use blocks of ``// single line comments``:
```cpp
void FFoo::Bar()
{
    // Number of shares must always be 42 because...
    const int32 NumSharesToBuy = 42;
}
```
