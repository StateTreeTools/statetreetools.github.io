---
title: Wait For Gameplay Tag Added
---

# Wait For Gameplay Tag Added

**Plugin:** StateTreeToolsGameplayAbilitySystem
**Category:** Gameplay Tags

Keeps the task running until the specified gameplay tag is added to the bound actor's Ability System Component. When the tag is detected the **On Tag Changed** delegate dispatcher fires and the configured **Output Event** is dispatched, then the task succeeds.

---

## Configuration

### Actor *(input)*
Bind this to the actor whose Ability System Component should be monitored for the tag.

### Tag
The `FGameplayTag` to watch for. The task succeeds when this tag is added to the ASC.

### On Tag Changed *(output)*
A `FStateTreeDelegateDispatcher` that fires when the tag is added. Bind this output to other parts of the tree if you need to react to the change outside of normal task completion.

### Output Event
A `FStateTreeTools_OutputEventAction` that configures an optional StateTree event to dispatch automatically when the tag is added.

---

## Unreal Engine Version Notes

> ⚠️ **Recompile required when switching engine versions.** This task's tick behaviour and completion tracking differ between UE 5.5 and 5.6. Any StateTree containing Wait For Gameplay Tag Added must be recompiled in the target engine version for it to behave correctly.

### UE 5.6 and later
- Task completes asynchronously the instant the tag is added — no per-frame tick is needed.
- The **On Tag Changed** delegate dispatcher fires at the exact moment the tag change is detected.
- The **Output Event** is dispatched immediately when the callback fires.
- The task is not considered for completion tracking (`bConsideredForCompletion = false`); completion is driven entirely by the async callback.

### UE 5.5
- Task completes via a per-frame tick that polls a shared flag set by the tag-change callback. There is a one-frame delay between the tag being added and the task completing.
- The **On Tag Changed** delegate dispatcher output appears in the details panel but is non-functional — `FStateTreeDelegateDispatcher` does not exist before UE 5.6 and the field compiles as a no-op placeholder.
- The **Output Event** still dispatches its configured StateTree event (fired from the tick).
- The task uses standard completion tracking (`bConsideredForCompletion = true`).

---

## Error Handling

If the actor is invalid or has no Ability System Component the task will fail. You can control whether a failure causes the task to return **Failed** or **Succeeded** using the **Error Means Failure** option.

[← Back to Abilities](/tasks/abilities) · [← Back to home](/)
