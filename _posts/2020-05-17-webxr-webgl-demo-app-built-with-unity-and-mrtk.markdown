---
layout: post
title:  WebXR demo app built with Unity and MRTK
date:   2020-05-17
image:  '/images/WebXR-demo-app-built-with-Unity-and-MRTK.jpg'
tags:   WebGL WebXR Unity MRTK Javascript
hn_id: 23235013
hn_link: https://rufus31415.github.io/webxr-webgl-demo-app-built-with-unity-and-mrtk.html
repo: Simple-WebXR-Unity
---

EDIT :

⚠️⚠️⚠️ This article describes a test, please visit the official repository : [SimpleWebXR for Unity](https://github.com/Rufus31415/Simple-WebXR-Unity) ⚠️⚠️⚠️



This article describes how I add the WebXR target to the MRTK, allowing to do augmented reality in the web browser. This project is a proof of concept and not an industrial implementation ! I developed this demonstration over a long weekend in confinement.

Associated Github repository : [https://github.com/Rufus31415/MixedRealityToolkit-Unity-WebXR](https://github.com/Rufus31415/MixedRealityToolkit-Unity-WebXR/)

**Live demo** (please use [WebXR Viewer browser](https://apps.apple.com/us/app/webxr-viewer/id1295998056){:target="_blank"} app on iOS) : [https://rufus31415.github.io/sandbox/webxr-hand-interaction/](https://rufus31415.github.io/sandbox/webxr-hand-interaction/)

<p><iframe src="https://www.youtube.com/embed/msORgnO6R9U?cc_load_policy=1&rel=0" frameborder="0" allowfullscreen></iframe></p>


| | | |
|:-------------------------:|:-------------------------:|:-------------------------:|
|<img width="1604" src="https://raw.githubusercontent.com/Rufus31415/MixedRealityToolkit-Unity-WebXR/mrtk_webxr_prototype/Videos/move.gif"> |  <img width="1604" src="https://raw.githubusercontent.com/Rufus31415/MixedRealityToolkit-Unity-WebXR/mrtk_webxr_prototype/Videos/rotate.gif">|<img width="1604" src="https://raw.githubusercontent.com/Rufus31415/MixedRealityToolkit-Unity-WebXR/mrtk_webxr_prototype/Videos/scroll.gif">|
|<img width="1604" src="https://raw.githubusercontent.com/Rufus31415/MixedRealityToolkit-Unity-WebXR/mrtk_webxr_prototype/Videos/slider.gif">  |  <img width="1604" src="https://raw.githubusercontent.com/Rufus31415/MixedRealityToolkit-Unity-WebXR/mrtk_webxr_prototype/Videos/piano.gif">|<img width="1604" src="https://raw.githubusercontent.com/Rufus31415/MixedRealityToolkit-Unity-WebXR/mrtk_webxr_prototype/Videos/world-mapping.gif">|


# What is the Mixed Reality Toolkit ?

MRTK-Unity is a Microsoft-driven project that provides a set of components and features, used to accelerate cross-platform MR app development in Unity. Here are some of its functions:

* Provides the **basic building blocks for Unity development on HoloLens, Windows Mixed Reality, and OpenVR**.
* Enables **rapid prototyping** via in-editor simulation that allows you to see changes immediately.
* Operates as an **extensible framework** that provides developers the ability to swap out core components.
* **Supports a wide range of platforms**, including
  * Microsoft HoloLens
  * Microsoft HoloLens 2
  * Windows Mixed Reality headsets
  * OpenVR headsets (HTC Vive / Oculus Rift)
[read more...](https://microsoft.github.io/MixedRealityToolkit-Unity/){:target="_blank"}


# What is WebXR ?

The WebXR Device API provides access to input and output capabilities commonly associated with Virtual Reality (VR) and Augmented Reality (AR) devices. It allows you develop and host VR and AR experiences on the web. Today (May 2020), few browsers are currently WebXR compatible. [read more...](https://github.com/immersive-web/webxr/blob/master/explainer.md){:target="_blank"}


# What is Unity ?
[Unity](https://unity.com){:target="_blank"} is a 3D rendering engine and development environment to create video games, but also 3D experiences of augmented or virtual reality. It provides applications for Windows, MacOS, Linux, Playstation, Xbox, Hololens, WebGL...

WebGL is here the export format used.


# What is WebGL ?
WebGL allows to dynamically display, create and manage complex 3D graphical elements in a client's web browser. Most desktop and mobile browsers are now compatible with WebGL.


# How do you mix all these technologies?

MRTK is developed in Unity. I used here the example scene [Hand Interaction](https://microsoft.github.io/MixedRealityToolkit-Unity/Documentation/README_HandInteractionExamples.html){:target="_blank"}. I compiled it into WebGL with Unity, so I got javascript (as [WebAssembly](https://developer.mozilla.org/fr/docs/WebAssembly){:target="_blank"}).
Since Safari is not yet compatible with WebXR under iOS (I don't have an Android phone), I had to use the [XRViewer browser](https://apps.apple.com/us/app/webxr-viewer/id1295998056){:target="_blank"} developed by Mozilla. However, the implementation of WebXR is not standard because it was a test application. ([read more about XRViewer app...](https://github.com/mozilla-mobile/webxr-ios){:target="_blank"}.
I was inspired by the [examples provided by Mozilla](https://ios-viewer.webxrexperiments.com/){:target="_blank"}. I added the generated WebGL component on top of it. I use the WebXR javascript API to transmit the position of the smartphone to the Unity Main Camera.

Moreover, according to the Mozilla examples, I use [Three.js](https://threejs.org/){:target="_blank"} to display the world mapping mesh (in green).

# Touch screen

A new Input System Profile has been created to support multitouch as well as the mouse pointer. See [MyMixedRealityInputSystemProfile.asset](https://github.com/Rufus31415/MixedRealityToolkit-Unity-WebXR/blob/mrtk_webxr_prototype/Assets/MRTK/SDK/Profiles/HoloLens2/MyMixedRealityInputSystemProfile.asset){:target="_blank"} and [MyMixedRealityHandTrackingProfile.asset](https://github.com/Rufus31415/MixedRealityToolkit-Unity-WebXR/blob/mrtk_webxr_prototype/Assets/MRTK/SDK/Profiles/HoloLens2/MyMixedRealityHandTrackingProfile.asset){:target="_blank"}.


# WebGL canvas transparent background

The WebGL canvas is placed on top of the video canvas. The black background normally generated by Unity must be made transparent in order to view the video behind the holograms.

For this, a new [Camera Profile](https://github.com/Rufus31415/MixedRealityToolkit-Unity-WebXR/blob/mrtk_webxr_prototype/Assets/MixedRealityToolkit.Generated/CustomProfiles/MyMixedRealityCameraProfile.asset){:target="_blank"} has been created with a solid color (black background) and 100% transparency.

In addition, a [jslib plugin](https://github.com/Rufus31415/MixedRealityToolkit-Unity-WebXR/blob/mrtk_webxr_prototype/Assets/WebXR/TransparentBackground.jslib){:target="_blank"} has been written to apply this transparency which is not supported by default in WebGL.

``` javascript
var LibraryGLClear = {
	glClear: function(mask) {
		if (mask == 0x00004000) {
			var v = GLctx.getParameter(GLctx.COLOR_WRITEMASK);
			if (!v[0] && !v[1] && !v[2] && v[3]) return;
		}
		GLctx.clear(mask);
	}
};
mergeInto(LibraryManager.library, LibraryGLClear);
```

Finally, after generation, the ```Build/build.json``` file must be modified to replace ```"backgroundColor": "#000000"``` by ```"backgroundColor": "transparent"```. This operation could be done automatically in post-build.


# HTML
A [WebGL export template](https://github.com/Rufus31415/MixedRealityToolkit-Unity-WebXR/tree/mrtk_webxr_prototype/Assets/WebGLTemplates/WebXR){:target="_blank"} is used to generate the web page to get the final application. This template was inspired by the [World Sensing](https://github.com/mozilla/webxr-polyfill/blob/master/examples/sensing/index.html){:target="_blank"} example from Mozilla. It manages the display of the video, the start of the WebXR session and the display in green of the mesh from the world scanning. I added on the video the WebGL canvas generated by Unity.


# Communication between WebXR and Unity

The position of the smartphone is in a Camera Three.js object. It is recovered in loop and transmitted to the Unity camera with the following code:

[Index.html](https://github.com/Rufus31415/MixedRealityToolkit-Unity-WebXR/blob/06d50cbc4fc72e8bfead6bf00bd19511868e2c94/Assets/WebGLTemplates/WebXR/index.html#L61){:target="_blank"} :
``` javascript
var pose = new THREE.Vector3();
var quaternion = new THREE.Quaternion();
var scale = new THREE.Vector3();

engine.camera.matrix.decompose(pose, quaternion, scale);

var coef = 1/0.4; // Three.js to Unity length scale
var msg = {x: pose.x * coef, y: pose.y * coef, z: -pose.z * coef, _x: quaternion._x, _y: quaternion._y, _z: quaternion._z, _w: quaternion._w, crx: -1, cry: -1, crz: 1, fov:40};
var msgStr = JSON.stringify(msg);

unityInstance.SendMessage("JSConnector", "setCameraTransform", msgStr); // send message to Unity (only 1 argument possible)
```


[JSConnector.cs](https://github.com/Rufus31415/MixedRealityToolkit-Unity-WebXR/blob/mrtk_webxr_prototype/Assets/WebXR/JSConnector.cs){:target="_blank"} :
``` cs
public void setCameraTransform(string msgStr) {
	var msg = JsonUtility.FromJson<JSCameraTransformMessage>(msgStr);

	Camera.main.transform.position = new Vector3(msg.x, msg.y, msg.z);

	var quat = new Quaternion(msg._x, msg._y, msg._z, msg._w);
	var euler = quat.eulerAngles;

	Camera.main.transform.rotation = Quaternion.Euler(msg.crx * euler.x, msg.cry * euler.y, msg.crz * euler.z);

	Camera.main.fieldOfView = msg.fov;
}
```

Some settings, such as scale change and FOV are hard written and adapted to my iPhone 7 for this POC. 

To be more efficient, the access to the WebXR API should be done in a jslib plugin so that the code is integrated to the WebGL build, this would avoid the heaviness of Three.js, the lags of the "sendMessage" function, and the decoding/encoding of json in string. Also, it would be necessary to think about how to transmit the good parameters of FOV and scaling to Unity. The current code is clearly inefficient, but it is a quick working proof of concept.


# Compilation

WebGL compilation is performed by Unity using the [IL2CPP](https://docs.unity3d.com/Manual/IL2CPP.html) scripting backend then the [Emscripten](https://emscripten.org){:target="_blank"} WebAssembly compiler.
Note that the [ARSessionOrigin.cs](https://github.com/Rufus31415/MixedRealityToolkit-Unity-WebXR/blob/mrtk_webxr_prototype/Library/PackageCache/com.unity.xr.arfoundation%401.5.0-preview.6/Runtime/AR/ARSessionOrigin.cs){:target="_blank"} file in the AR Foundation package was voluntarily committed because it includes modifications to be compilable in WebGL.

As mentioned above, after compilation, the Builb/build.json file must be modified for supporting transparency.

Finally, to avoid warnings at runtime about browser incompatibility, the code generated in ```UnityLoader.js``` should be modified by replacing: ```UnityLoader.SystemInfo.mobile``` by ```false``` and ```["Edge", "Firefox", "Chrome", "Safari"].indexOf(UnityLoader.SystemInfo.browser)==-1``` by ```false```.


# Browser compatibility

To date (May 2020), the [XRViewer application](https://apps.apple.com/us/app/webxr-viewer/id1295998056){:target="_blank"} is the only browser that offers a WebXR implementation on iOS (Safari is not yet compatible). But this implementation is a draft as explained [here](https://github.com/mozilla-mobile/webxr-ios#warning-experimental-exploration-of-the-future-of-webxr){:target="_blank"}. It would not be complicated to make this example compatible with Chrome for Android, or Chrome or Firefox for desktop. This would allow to generate Unity scenes compatible with Android and virtual reality devices like WMR, Oculus, OpenVR, or Vive. Maybe I'll work on it if needed 😀.

If you are using a browser other than XRViewer, you will get an error popup indicating that WebXR is not supported. But you can then enjoy the WebGL scene with the mouse.


With another browser :

![](https://raw.githubusercontent.com/Rufus31415/MixedRealityToolkit-Unity-WebXR/mrtk_webxr_prototype/Videos/desktop.gif)


With XRViewer :

![](https://raw.githubusercontent.com/Rufus31415/MixedRealityToolkit-Unity-WebXR/mrtk_webxr_prototype/Videos/piano.gif)

