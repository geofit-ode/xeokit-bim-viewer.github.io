<!doctype html>
<html>
<body>
<canvas id="myCanvas"></canvas>
<div id="info">
    <ul>
        <li>
            <div id="time">Loading JavaScript modules...</div>
        </li>

    </ul>
</div>
</body>
<script type="module">

    //------------------------------------------------------------------------------------------------------------------
    // Import the modules we need for this example
    //------------------------------------------------------------------------------------------------------------------

    import {Viewer} from "../src/viewer/Viewer.js";
    import {XKTLoaderPlugin} from "../src/plugins/XKTLoaderPlugin.js";

    //------------------------------------------------------------------------------------------------------------------
    // Create a Viewer, arrange the camera
    //------------------------------------------------------------------------------------------------------------------

    const viewer = new Viewer({
        canvasId: "myCanvas",
        transparent: true
    });

    viewer.scene.camera.eye = [10.45, 17.38, -98.31];
    viewer.scene.camera.look = [43.09, 0.5, -26.76];
    viewer.scene.camera.up = [0.06, 0.96, 0.16];

    //----------------------------------------------------------------------------------------------------------------------
    // Create an XKT loader plugin, load a model, fit to view
    //----------------------------------------------------------------------------------------------------------------------

    const xktLoader = new XKTLoaderPlugin(viewer);

    var t0 = performance.now();

    document.getElementById("time").innerHTML = "Loading model...";

    const model = xktLoader.load({
        id: "myModel",
        src: "1261.xkt",
        metaModelSrc: "1261.json", // Creates a MetaObject instances in scene.metaScene.metaObjects
        edges: true
    });

    model.on("loaded", () => {
        var t1 = performance.now();
        document.getElementById("time").innerHTML = "Model loaded in " + Math.floor(t1 - t0) / 1000.0 + " seconds<br>Objects: " + model.numEntities;
    });

</script>
</html>
