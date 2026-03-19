# StateTree Tools

StateTree Tools is a plugin for Unreal Engine 5 that extends the StateTree system with a large collection of ready-made tasks, making it faster to build AI and gameplay logic without writing custom C++.

## What's Included

### StateTreeToolsCore
General-purpose tasks for common game logic:

- **Audio** — play and stop sounds on actors
- **Animation** — trigger and control animation montages
- **Components** — enable, disable, and toggle visibility of components
- **Debug** — draw debug shapes in the world; flush persistent debug lines and strings on enter or exit
- **Niagara** — spawn and manage Niagara particle systems, including attached effects
- **Navigation** — find random reachable points within a radius for patrol and wandering behaviour
- **Gameplay | StateTree** — send events (with typed payloads) to StateTree components on any actor
- **Utilities | Events** — call Blueprint events and event dispatchers on actors by name, with parameters
- **Utilities | Properties** — set any exposed property on an actor or component by name
- **Utilities** — general string, math, and helper tasks

Many tasks support firing on **state enter**, **state exit**, or **both**, configurable per task.

Also includes a **Perception Event Forwarder** component that listens to an AI Perception component on the same actor and automatically sends StateTree events when a target is perceived or forgotten, with attitude and stimulus data in the payload.

### StateTreeToolsGameplayAbilitySystem
Tasks and components for projects using the Gameplay Ability System:

- **Abilities** — activate abilities by class, send gameplay events to actors
- A **GAS Event Forwarder** component that bridges GAS into the StateTree event system — map any gameplay event tag to a StateTree event tag, and optionally watch for gameplay tag additions and removals, all forwarded automatically as StateTree events with typed payloads.

## Native Gameplay Tags

StateTree Tools defines a set of built-in event tags you can use in "On Event" transitions:

| Tag | Fired by |
|-----|----------|
| `StateTreeTools.Events.Perception.TargetUpdated` | Perception Event Forwarder — target perceived or stimulus updated |
| `StateTreeTools.Events.Perception.TargetForgotten` | Perception Event Forwarder — target lost |
| `StateTreeTools.Events.TagAdded` | GAS Event Forwarder — watched gameplay tag added |
| `StateTreeTools.Events.TagRemoved` | GAS Event Forwarder — watched gameplay tag removed |
