
import {Viewer} from "../src/viewer/Viewer.js";
import {XKTLoaderPlugin} from "../src/plugins/XKTLoaderPlugin/XKTLoaderPlugin.js";

const viewer = new Viewer({
     canvasId: "myCanvas"
});

const xktLoader = new XKTLoaderPlugin(viewer);

const modelNode = xktLoader.load({
     id: "myModel",
     src: "./HolterTower.xkt",
     metaModelSrc: "./HolterTower.json",
     edges: true // Emphasise edges
});

modelNode.on("loaded", () => {

     const scene = viewer.scene;
     const camera = scene.camera;

     camera.eye = [-2.37, 18.97, -26.12];
     camera.look = [10.97, 5.82, -11.22];
     camera.up = [0.36, 0.83, 0.40];
});