<!DOCTYPE html>
<html lang="">
<head>
    <meta charset="utf-8"/>
    <meta content="width=device-width, initial-scale=1" name="viewport">
    <title>XKTLoaderPlugin - Loading an IFC Model from the File System</title>
    <link href="css/styles.css" rel="stylesheet" type="text/css"/>

   

</head>
<body>

<canvas id="myCanvas"></canvas>

<canvas id="myNavCubeCanvas"></canvas>

<div id="treeViewContainer"></div>

<div id="info">
    <h1>XKTLoaderPlugin - Loading an IFC4 Model from the File System</h1><br>
    <ul>
        <li>
            <div id="time">Loading JavaScript modules...</div>
        </li>
        <li>
            <a href="./../docs/class/src/viewer/Viewer.js~Viewer.html"
               target="_other">Viewer</a>
        </li>
        <li>
            <a href="./../docs/class/src/plugins/XKTLoaderPlugin/XKTLoaderPlugin.js~XKTLoaderPlugin.html"
               target="_other">XKTLoaderPlugin</a>
        </li>
        <li>
            <a href="./../docs/class/src/plugins/TreeViewPlugin/TreeViewPlugin.js~TreeViewPlugin.html"
               target="_other">TreeViewPlugin</a>
        </li>
        <li>
            <a href="./../docs/class/src/plugins/NavCubePlugin/NavCubePlugin.js~NavCubePlugin.html"
               target="_other">NavCubePlugin</a>
        </li>
        <li>
            <a href="http://openifcmodel.cs.auckland.ac.nz/Model/Details/316"
               target="_other">Model source</a>
        </li>
    </ul>
</div>

</body>

<script type="module">

    //------------------------------------------------------------------------------------------------------------------
    // Import the modules we need for this example
    //------------------------------------------------------------------------------------------------------------------

    import {Viewer} from "../src/viewer/Viewer.js";
    import {XKTLoaderPlugin} from "../src/plugins/XKTLoaderPlugin/XKTLoaderPlugin.js";
    import {AmbientLight} from '../src/viewer/scene/lights/AmbientLight.js';
    import {NavCubePlugin} from "../src/plugins/NavCubePlugin/NavCubePlugin.js";
    import {TreeViewPlugin} from "../src/plugins/TreeViewPlugin/TreeViewPlugin.js";

    //------------------------------------------------------------------------------------------------------------------
    // Create a Viewer, arrange the camera, tweak x-ray and highlight materials
    //------------------------------------------------------------------------------------------------------------------

    const viewer = new Viewer({
        canvasId: "myCanvas",
        transparent: true
    });

    viewer.cameraControl.followPointer = true;

    viewer.scene.camera.eye = [-66.26, 105.84, -281.92];
    viewer.scene.camera.look = [42.45, 49.62, -43.59];
    viewer.scene.camera.up = [0.05, 0.95, 0.15];

    viewer.scene.xrayMaterial.fillAlpha = 0.1;
    viewer.scene.xrayMaterial.fillColor = [0, 0, 0];
    viewer.scene.xrayMaterial.edgeAlpha = 0.4;
    viewer.scene.xrayMaterial.edgeColor = [0, 0, 0];

    viewer.scene.highlightMaterial.fill = false;
    viewer.scene.highlightMaterial.fillAlpha = 0.3;
    viewer.scene.highlightMaterial.edgeColor = [1, 1, 0];

    new AmbientLight(viewer.scene, {
        color: [0.35, 0.35, 0.2],
        intensity: 1.0
    });

    //------------------------------------------------------------------------------------------------------------------
    // Create a NavCube
    //------------------------------------------------------------------------------------------------------------------

    new NavCubePlugin(viewer, {
        canvasId: "myNavCubeCanvas",
        visible: true,
        size: 250,
        alignment: "bottomRight",
        bottomMargin: 100,
        rightMargin: 10
    });

    //------------------------------------------------------------------------------------------------------------------
    // Create an IFC structure tree view
    //------------------------------------------------------------------------------------------------------------------

    const treeView = new TreeViewPlugin(viewer, {
        containerElement: document.getElementById("treeViewContainer"),
        autoExpandDepth: 3 // Initially expand tree three nodes deep
    });

    //------------------------------------------------------------------------------------------------------------------
    // Load model
    //------------------------------------------------------------------------------------------------------------------

    const xktLoader = new XKTLoaderPlugin(viewer);

    const model = xktLoader.load({
        id: "myModel",
        src: "./../assets/models/xkt/HolterTower/HolterTower.xkt",
        metaModelSrc: "./../assets/metaModels/HolterTower/HolterTower.json", // Creates a MetaObject instances in scene.metaScene.metaObjects
        excludeUnclassifiedObjects: true,
        edges: true
    });

    const t0 = performance.now();

    document.getElementById("time").innerHTML = "Loading model...";

    model.on("loaded", function () {
        const t1 = performance.now();
        document.getElementById("time").innerHTML = "Model loaded in " + Math.floor(t1 - t0) / 1000.0 + " seconds<br>Objects: " + model.numEntities;
    });

    //------------------------------------------------------------------------------------------------------------------
    // Mouse over entities to highlight them
    //------------------------------------------------------------------------------------------------------------------

    var lastEntity = null;

    viewer.scene.input.on("mousemove", function (coords) {

        var hit = viewer.scene.pick({
            canvasPos: coords
        });

        if (hit) {

            if (!lastEntity || hit.entity.id !== lastEntity.id) {

                if (lastEntity) {
                    lastEntity.highlighted = false;
                }

                lastEntity = hit.entity;
                hit.entity.highlighted = true;
            }
        } else {

            if (lastEntity) {
                lastEntity.highlighted = false;
                lastEntity = null;
            }
        }
    });

</script>
</html>
