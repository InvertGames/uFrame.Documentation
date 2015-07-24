# Observable Examples

Inspired by http://rxwiki.wikidot.com/101samples

Please feel free to add more!

## Simple RX Time. Should be self explanatory.

```csharp
this.UpdateAsObservable().Subscribe(_=>{ Debug.Log("THIS IS CALLED EVERY FRAME") });
```

```csharp
this.UpdateAsObservable().Where(_=>Input.GetMouseButtonDown(0)).Subscribe(_=>{ Debug.Log("THIS IS CALLED when the mouse button is down") });
```

```csharp
this.UpdateAsObservable().Where(_=>Input.GetMouseButtonDown(0)).Throttle(TimeSpan.FromSeconds(1)).Subscribe(_=>{ Debug.Log("This is only called once per second. This keeps many clicks from firing this to quickly") });
```

## Once per second, execute command to damage player

```csharp
Observable.Interval(TimeSpan.FromMilliseconds(1000))
	.Subscribe(_ => {
			ExecutedamagePerSecondTick();
	}).DisposeWhenChanged(Player.healthStateProperty);
```

## Time based functionality without co-routines in controller to trigger waves.

```csharp
if (state is Wave)
{
	// Wait for the alloted amount of time
	Observable.Interval(TimeSpan.FromSeconds(wavesFPSGame.SpawnWaitSeconds))
	.Where(_ => wavesFPSGame.WavesState is Wave)
	.Take(wavesFPSGame.KillsToNextWave)
	.Subscribe(l => {
		SpawnEnemy();
	});
}
```

## uGUI events as observables. A button as an event stream in your view.

```csharp
Button poorButton;  // Our button.

poorButton.AsClickObservable()
	.Where(_ => YOUR FILTER )
	.Throttle(_ => PREVENT SPAMMING )
	.DelayFrame( FRAMES TO DELAY )
	.SkipWhile( SKIP FIRST TAPS IF CONDITION IS NOT MET )
	.Subscribe(_ =>
	{
		USE LIKE SIMPLE OBSERVABLE
	});
```

## Flash sprites on and off rapidly.

```csharp
	var sprites = GetComponentsInChildren<SpriteRenderer>().ToList ();

	if(value){
		Observable.Interval(TimeSpan.FromMilliseconds(100))
				.Subscribe(l =>
				{
					if (Character.isInvulnerable)
						sprites.ForEach(s => s.enabled = 1 % 2 == 0);
				}).DisposeWhenChanged(Character._isInvulnerableProperty);
		}
		else
		{
			sprites.ForEach(s => s.enabled = true);
		}

```

## Bind to state machines last state

```csharp
{StateMachineProperty}Property.Subscribe(_=>{  {StateMachineProperty}Property.LastState });
```

## Setting up a subscription for when a state changes

```csharp
// Do something on state changes
myViewModel.myStateMachine.StateMachine
    .Subscribe(newState => {
       // Do something now that state has changed
    });

// Listen for a single state change then stop listening
myViewModel.myStateMachine.StateMachine
    .Single()
    .Subscribe(newState => {
       // Do something once
    });
```

## Bind to various trigger and collision events.

Filter by the type of view specified.

Triggers.
```csharp
this.BindViewTriggerWith<PlayerViewBase>(CollisionEventType.Enter, (_ )=> ExecutePlayerEntered() );
```

Collision events. Requires at least one non-kinematic rigidbody.
http://docs.unity3d.com/ScriptReference/Collider.OnCollisionEnter.html

```csharp
this.BindViewCollisionWith<PlayerViewBase>(CollisionEventType.Enter, (_ )=> ExecutePlayerEntered() );
```

Bind to various trigger and collision events, and subscribe to changes on the ViewModel of the object that entered your trigger.

On the view attached to your game object with the trigger:

```csharp
this.BindViewTriggerWith<ThingyView>(CollisionEventType.Enter, _thingyView => ExecuteSubscribeToViewModel(_thingyView.ViewModelObject as ThingyViewModel));

this.BindViewTriggerWith<ThingyBarView>(CollisionEventType.Exit, _thingyView => ExecuteUnsubscribeFromThingy(_thingyView.ViewModelObject as ThingyViewModel));
```

In your element's controller:

```csharp
public override void SubscribeToViewModel (ThisViewModel this, ThingyViewModel arg)
{
    base.SubscribeToThingy (this, arg);

    this.Thingy = arg;
    this.Thingy.StateProperty
    .Where(s => s == ThingyState.Dead)
    .Subscribe( _ => ExecuteCommand(this.Idle))
    .DisposeWhenChanged(this.ThingyProperty);
}

public override void UnsubscribeFromViewModel (ThisViewModel this, ThingyViewModel arg)
{
    base.UnsubscribeFromViewModel (this, arg);

    this.Thingy = null;
}
```
