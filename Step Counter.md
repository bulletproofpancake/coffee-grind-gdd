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

3.  Add the `StepCounter` sensor to the `InputSystem`.

```csharp
using UnityEngine.InputSystem;

#if PLATFORM_ANDROID
	// Place in Start
	if(StepCounter.current == null)
	{
		InputSystem.A
	}
#endif
```