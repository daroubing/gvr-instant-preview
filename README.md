# Daydream Instant Preview for Unity

Instant Preview allows you to skip the build process and test your VR app instantly on your device, significantly improving iteration time. 

Please note, we do not accept pull requests.

## Instructions:
* Install the Instant Preview application to your device. From the command line in the folder containing the apk: adb install -r InstantPreview.apk
* Ensure your build platform is set to Android in Unity.
* Import the gvr-unity-sdk.unitypackage if your project does not have it already.
* Import the Instant Preview .unitypackage that corresponds to your GVR Unity SDK version:
  * The GVR Unity SDK version can be found in GvrUnitySdkVersion.cs
  * The version also displays in the Unity console when you preview.
  * Instant Preview currently supports version 1.40 and 1.50 of the GVR Unity SDK.
* Take the GvrInstantPreviewMain.prefab from the InstantPreview/Assets folder and add it in your scene hierarchy. It persists across scene transitions, so you can add it to your entry point scene root. Configure the script settings if desired.
* Connect your phone via USB 3.0 and launch the app.

## FAQ
Q: The app says it is connected but never starts streaming.
A: Another instance of Unity might be using Instant Preview. Only one instance a Unity using Instant Preview can be used at once right now. Please check that you have set your platform to Android, you have imported both the gvr-unity-sdk and InstantPreview .unitypackage to your project, and you have added the GvrInstantPreviewMain prefab to your scene. 

Q: My controller isn't working. Help!
A: Check that you have no exceptions in the Console when running. There is a known issue with the gvr-unity-sdk if you switch to Android after importing the package where a null material on the controller will stop the controller appearing in the editor.

Notes
* Only supports Windows 64bit and OSX.
* Default quality is set to very high quality. If you encounter low performance, reduce the bitrate and resolution to match your system.
* Neck model scale is not currently being applied.

## About the Unity script

**InstantPreview.cs** works by adding two "eye cameras" parented to the main camera in a scene. The eye cameras render to a texture, which is then streamed to the Instant Preview app. The app will send head poses which the script uses to adjust the orientation and projection parameters of the main camera. 

In the case that the main camera is removed or absent, Instant Preview will have nowhere to place its eye cameras and thus not be able to stream anything.

This setup should work well for most scenes, but more complex rendering setups might cause problems. If a scene uses multiple cameras, Instant Preview will only attach eye cameras to the one tagged as the main camera and ignore the other ones. To incorporate additional cameras into Instant Preview, the script must be modified so that additional pairs of eye cameras composite the views of the auxiliary cameras into the same render texture which is being streamed to the app. 

If the previewed scene appears different from expected, verify everything is rendering to the Instant Preview render texture as it should.

You should disable vsync for best streaming performance. Edit -> Project Settings -> Quality and set V Sync Count to Don't Sync.
