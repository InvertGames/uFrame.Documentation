# uFrame Entity Component System
Welcome to the uFrame Entity Component Documentation!

## Overview
- [Components](API/Components.md)
- [Groups](API/Groups.md)
- [Systems](API/Systems.md)
- [Handlers](API/Handlers.md)
- [Events](API/Events.md)
- [Descriptors](API/Descriptors.md)
- [The Entity Component](API/EntityComponent.md)
- [Entity Proxies](API/EntityProxies.md)
- [Create Custom Actions](API/CreateCustomActions.md)
- [Get More Actions](ThirdPartyActions.md)

## Developer API
The Developer API documentation is located [here](CodeApi/Overview.md)

## Tutorials
- [Getting Started - Project Setup](https://youtu.be/uxivyGL5StA)
- [Creating Components - Hello World Example](https://youtu.be/vGRgN-MZEAA)
- [Creating And Using Groups - Component Matching](https://youtu.be/5EwZWWfpBBI)
- [Creating And Using Groups 2 - Expression Filtering](https://youtu.be/iMjs26dA2rg)
- [Creating And Using Custom Events](https://youtu.be/h_s-l30rNe0)
- [Creating Custom Actions](https://youtu.be/AuockvC5Cys)
- [Creating Code Handlers](https://www.youtube.com/watch?v=tloQJ2viEmI)

## API
Classes  | Description
------------- | -------------
EcsSystem  | Used for listening to events, and invoking handlers.
EcsComponent | The base class for all components, it is an extended version of MonoBehaviour.
Code Handlers  | Used to actually execute an action or sequence of actions invoked by a system.
