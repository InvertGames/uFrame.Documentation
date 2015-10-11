# Theory
## Why Use an Entity-Component-System?


## The Normal Way
To understand how ECS works and why it can make your life so much easier first lets examine a simple psuedo example of how you might have created a component in unity.

```cs
public class Player : MonoBehaviour
{
  public bool IsAlive = true;
  public void KillPlayer() {
    IsAlive = false;
    Destroy(this);
    Instantiate("PlayerDeathEffect");
    PlaySound("PlayerDeathSound");
  }
}
```
Take into account that this a very simple example.  At first glance this would look like there aren't any problems and that its very straight-forward, so I'll point out a few pitfalls to this approach.

- Anytime we want to change something about what happens when the player dies must belong to the KillPlayer method.
- Three completely seperate ideas are handled in the same location (Destroying, Visual FX, and Sound FX).
- If we want to kill the player, we must obtain a reference to this player and invoke its method.  
- The fact that all functionality is in one place isn't very modular, if you wanted to re-use this player with another game, or a different game setup (rules..etc) it'll quick look like spaghetti spilt on the floor.

## The ECS Way
So now the fun part, lets take this same example and employ the concept of ECS into it.
##### The Component - Data Only
```cs
public class Damageable : EcsComponent {
  public int Health;
}

```
##### The Event - Data Only
The event defines a contract for executing an operation, or a notification for when something has happened. Usually any kind of game level operation such as (KillPlayer, CastSpell) could really be both an event and an operation.

```cs
public KillPlayerEvent {
  Damageable DamageableComponent;
}
```


##### The System - The Behavior
```cs
public class DamageSystem : EcsSystem {
  public void Init() {
    ListenFor<DeathEvent>(KillPlayerFunction);
  }
  public void KillPlayer(KillPlayerEvent eventData) {

  }
}
```
