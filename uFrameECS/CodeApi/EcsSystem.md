# EcsSystem
This is the base class for all systems.  It derives from SystemServiceMonoBehaviour which is part of the uFrame Kernel

## Properties
Name | Type | Description
-----|------|------------
ComponentSystem|Property|Access to the component system.  Use this to get/register components or groups.


## Setup Method
The setup method is used to register groups/components, and setup event listeners using EventAggregator
### Example
```cs
public override void Setup() {
        base.Setup();
        EnemyAIManager = ComponentSystem.RegisterComponent-EnemyAI-();
        RandomRotationManager = ComponentSystem.RegisterComponent-RandomRotation-();
        ProjectileManager = ComponentSystem.RegisterComponent-Projectile-();
        SpawnWithRandomXManager = ComponentSystem.RegisterComponent-SpawnWithRandomX-();
        DestroyOnCollisionManager = ComponentSystem.RegisterComponen-DestroyOnCollision-();
        this.OnEvent-uFrame.ECS.OnTriggerEnterDispatcher-().Subscribe(_=>{ HandleDestroyOnCollisionFilter(_); }).DisposeWith(this);
        RandomRotationManager.CreatedObservable.Subscribe(BeginRandomRotationComponentCreatedFilter).DisposeWith(this);
        ProjectileManager.CreatedObservable.Subscribe(ProjectileCreatedComponentCreatedFilter).DisposeWith(this);
        SpawnWithRandomXManager.CreatedObservable.Subscribe(SetRandomPositionComponentCreatedFilter).DisposeWith(this);
        this.OnEvent-uFrame.ECS.OnCollisionEnterDispatcher-().Subscribe(_=>{ HazardSystemOnCollisionEnterDispatcherFilter(_); }).DisposeWith(this);
}
```
## Loaded Method
Invoked when all EcsSystem setup methods have been invoked
##### NOTE: This documentation has been generated, any edits will likely be overriden, to modify this, modify the doc comments in the source code.
