---
title: ReleaseNotes
description: release nots of the current MRTK version
author: polar-kev
ms.author: kesemple
ms.date: 01/12/2021
ms.localizationpriority: high
keywords: Unity,HoloLens, HoloLens 2, Mixed Reality, development, MRTK,
---

# Microsoft Mixed Reality Toolkit 2.5.1 release notes

- [What's new](#whats-new)
- [Breaking changes](#breaking-changes)
- [Updating guidance](../updates-deployment/Updating.md#upgrading-to-a-new-version-of-mrtk)
- [Known issues](#known-issues)

> [!IMPORTANT]
> There is a known compiler issue that impacts applications built for Microsoft HoloLens 2 using
> ARM64. This issue is addressed in the forthcoming 16.8 update for Visual Studio 2019. Until the
> update is available, please import the `com.microsoft.mixedreality.toolkit.tools` package to apply
> a workaround.

## What's new

### Package dependency errors fixed

This release fixes incorrect inter-package file dependencies (ex: files in Standard Assets no longer incorrectly reference files in Foundation). Version 2.5.1 also adds an explicit dependency on Text Mesh Pro.

### Standard Assets package shaders copied to Assets/MRTK/Shaders

When the Standard Assets package is installed via UPM, the shaders will be copied to the Assets/MRTK/Shaders folder so that they will no longer be immutable. This resolves the issue of shaders updated for the Universal Render Pipeline (URP) reverting the legacy behavior the next time the project is loaded.

### Fixed teleport cursor sticking to hands

This release fixes an [issue](https://github.com/microsoft/MixedRealityToolkit-Unity/issues/8755) where the teleport destination cursor can stick to hand visuals.

## Breaking changes

There are no breaking changes since version 2.5.0.

## Known issues

### Some Mixed Reality Toolkit Standard Shader features require the Foundation package

When imported via the Unity Package Manager, the MRTK Standard Shader utilities scripts (ex: HoverLight.cs) are not co-located with the shader in the Standard Assets package. To access this functionality, applications will require the Foundation package to be imported.

### CameraCache may create a new camera on shutdown

In some situations (e.g. when using the LeapMotion provider in the Unity Editor), it is possible for the CameraCache to re-create the MainCamera on shutdown. Please see [this issue](https://github.com/microsoft/MixedRealityToolkit-Unity/issues/8459) for more information.

### FileNotFoundException when examples are imported via Unity Package Manager

Depending on the length of the project path, importing examples via Unity Package Manager may generate FileNotFoundException messages in the Unity Console. The
cause of this is the path to the "missing" file being longer than MAX_PATH (256 characters). To resolve, please shorten the length of the project path.

### No spatializer was specified. The application will not support Spatial Sound

A "No spatializer was specified" warning will appear if an audio spatializer is not configured. This can occur if no XR package is installed, as Unity includes spatializers in these pacakges.

To resolve, please ensure that:

- **Window** > **Package Manager** has one or more XR packages installed
- **Mixed Reality Toolkit** > **Utilities** > **Configure Unity Project** and make a selection for **Audio Spatializer**

  ![Select Audio Apatializer](../features/images/release-notes/SpatializerSelection.png)

### NullReferenceException: Object reference not set to an instance of an object (SceneTransitionService.Initialize)

In some situations, opening `EyeTrackingDemo-00-RootScene` may cause a NullReferenceException in the Initialize method of the SceneTransitionService class.
This error is due to the Scene Transition Service's configuration profile being unset. To resolve, please use the following steps:

- Navigate to the `MixedRealityToolkit` object in the Hierarchy
- In the Inspector window, select `Extensions`
- If not expanded, expand `Scene Transition Service`
- Set the value of `Configuration Profile` to **MRTKExamplesHubSceneTransitionServiceProfile**

<img src="../features/images/release-notes/FixSceneTransitionProfile.png" width="500px" alt="Fix Scene Transition Profile">

### Oculus Quest

There is currently a known issue for using the [Oculus XR plugin with when targeting Standalone platforms](https://forum.unity.com/threads/unable-to-start-oculus-xr-plugin.913883/).  Check the Oculus bug tracker/forums/release notes for updates.

The bug is signified with this set of 3 errors:

![Oculus XR Plugin Error](https://forum.unity.com/attachments/erori-unity-png.644204/)

### UnityUI and TextMeshPro

There's a known issue for newer versions of TextMeshPro (1.5.0+ or 2.1.1+), where the default font size for dropdowns and bold font character spacing has been altered.

![TMP image](https://user-images.githubusercontent.com/68253937/93158069-4d582f00-f6c0-11ea-87ad-94d0ba3ba6e5.png)

This can be worked around by downgrading to an earlier version of TextMeshPro. See [issue #8556](https://github.com/microsoft/MixedRealityToolkit-Unity/issues/8556)
for more details.
