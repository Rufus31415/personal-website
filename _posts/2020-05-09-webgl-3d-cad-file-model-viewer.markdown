---
layout: post
title:  WebGL 45+ 3D file formats viewer
date:   2020-05-09
image:  '/images/WebGL-3D-CAD-model-viewer.jpg'
tags:   WebGL WebXR 3D Javascript
hn_id: 23141506
hn_link: https://rufus31415.github.io/sandbox/3d-viewer/
repo: react-webgl-3d-viewer-demo
---

This 3D viewer is made purely of JavaScript that runs locally in a person's browser. It allows you to load 45+ 3D model formats.

Demo: [https://rufus31415.github.io/sandbox/3d-viewer](https://rufus31415.github.io/sandbox/3d-viewer){:target="_blank"}

It's based on WebGL and WebAssembly technologies, as well as the Unity 3D rendering engine and the Assimp library. It also relies on a lot of code that I wrote myself.

This demo is entirely developed in React and bootstrapped with "Create React App". It allows you to load your own custom models as well as my provided demo models.

It has been tested with:
- Firefox on Windows
- Chrome on Windows
- Edge on Windows
- Safari on iOS (**note:** you should disable WebGL 2.0 for your Safari browser in your iOS Setttings under Experimental Features)

If WebGL 2.0 is not supported by your browser, the rendering is automatically toggled to WebGL 1.0.

Unfortunately, I can't publish the code that decodes the 3D formats because I had to buy several proprietary libraries and can only distribute the compiled binaries.

You can browse the list of supported formats displayed in the panel to the left. The long text is often taken from Wikipedia. The provided 3D models either come from free models of Assimp or self-created models made by me.

You can also load custom 3D models by uploading your own model or by inserting a online URL. If one of your models doesn't load, please email it to me at [rufus31415@gmail.com](mailto:rufus31415@gmail.com) for further debugging.

# Supported Formats

- 3DS
- 3MF
- AC
- AMF
- ASE
- B3D
- BLENDER
- BVH
- C4D
- COB
- Collada
- CSM
- DXF
- FBX
- glb
- glTF
- HMP
- IFC
- IRR
- JT
- LWO
- LWS
- LXO
- M3D
- MD2
- MD3
- MD4
- MD5
- MDL
- MS3D
- NFF
- OBJ
- OFF
- OpenGEX
- PLY
- Q3D
- RAW
- SIB
- SMD
- STEP
- STL
- TER
- X
- X3D
- XGL

# Roadmap

- Animation support
- Extending the support of the STEP format
- Add format detection if no extension is specified
- Add export to certain formats to use it as a conversion tool
- Add the possibility to view the model in augmented reality on compatible platforms
- Make it possible to manipulate the camera and add several models, via javascript
- Obtain via javascript the tree structure of the model
- Improve the handling of external resources (texture files, image...)
- Add other formats (like Solidworks, Creo, Catia, ...)

I don't know yet if I will continue to develop this viewer to make it a separate product: it will depend on whether it gains sufficient popularity.

If it does, I could make it a separate React component that can be installed via the package-manager: NPM.

i.e.

### NPM:

``` shell
npm install my-super-3d-viewer-react 
```

### JavaScript:

``` js
import Viewer from 'my-super-3d-viewer-react'

export default function App() {

    // path to my 3D file or binaries of my file
    const file = "https://ballcuber.github.io/assets/models/ballcuber.glb"

    const onViewerReady = () => {
        // Viewer is ready to use
    }

    const onViewerLoaded = () => {
        // The file has loaded successfully
    }

    const onViewerError = () => {
        // An error has occurred
    }

    return (
          <Viewer
            file={file}
            onReady = {onViewerReady}
            onLoaded = {onViewerLoaded}
            onError = {onViewerError}
          />
    );
}
```

# Final Comments

- Why not also isolate the loader to make it an extension to other 3D engines like Three.js
- Why not buy a Unity Pro subscription to remove the Unity watermark at runtime
- I would also like to reduce the size of the build: it's currently ~8mb
- I hope you'll like it, don't hesitate to ask me for improvements or ideas
