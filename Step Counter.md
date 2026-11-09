Tracking steps can be implemented in Unity by using the [StepCounter Sensor](https://docs.unity3d.com/Packages/com.unity.inputsystem@1.20/api/UnityEngine.InputSystem.StepCounter.html) in Unity's Input System. As per the documentation, this gives us the number of steps taken by the user since the last reboot while activated. Upon the first time that the value from the sensor was read, it gave a value of around 62,400 and increasing to 62,600 during testing. However, this is not the same number of steps I have taken that day which was around 11,000. This leads to the idea that **a step counter for the current session** should be implemented.

With that said, there are a few things to consider when implementing the step counter:

1. Make sure that the permission to `ACTIVITY_RECOGNITION` is added in the AndroidManifest by selecting **Export Project** during the build phase, opening this project in Android Studio, then adding the permission to `unityLibrary/manifests/AndroidManifest.xml`.

```xml
<uses-permission android:name="android.permission.ACTIVITY_RECOGNITION" />
```

2. Request the permission for the `ACTIVITY_RECOGNITION` in the Unity side of the game.

```csharp
#if PLATFORM_ANDROID
	using UnityEngine.Android; // add Android namespace for Permission class

	// Place in Start
	if(!Permission.HasUserAuthorizedPermission("android.permission.ACTIVITY_RECOGNITION"))
	{
		Permission.RequestUserPermission("android.permission.ACTIVITY_RECOGNITION");
	}
#endif
```

3.  Add the `StepCounter` sensor to the `InputSystem` and enable it.

```csharp
using UnityEngine.InputSystem;

#if PLATFORM_ANDROID
	// Place in Start
	if(StepCounter.current == null)
	{
		InputSystem.AddDevice<StepCounter>();
	}
	
	InputSystem.EnableDevice(StepCounter.current);
#endif
```

4. Read the value of `StepCounter` by calling `StepCounter.current.stepCounter.ReadValue()`, this returns an integer.

```csharp
private void Update()  
{  
    _label.text = StepCounter.current.enabled ? $"Steps: {StepCounter.current.stepCounter.ReadValue():000}" :"StepCounter is Paused.";  
    if(!StepCounter.current.enabled) InputSystem.EnableDevice(StepCounter.current);  
}
```

>[!NOTE]
> I am unsure of which statement made this work prior to previous setups. I have a hunch that it might be `InputSystem.AddDevice<StepCounter>()`, but I am also leaving the usage of having `InputSystem.EnableDevice(StepCounter.current)` as it might become useful in the future.   

# References
- https://discussions.unity.com/t/input-system-unresponsive-android-accelerometer-and-stepcounter/904467
- https://discussions.unity.com/t/stepcounter-readvalue-always-returns-zero/841471/3
- https://discussions.unity.com/t/help-with-sensors-using-the-inputsystem/796185
- https://discussions.unity.com/t/how-do-i-track-my-user-s-steps-while-app-is-minimised/351827/2
