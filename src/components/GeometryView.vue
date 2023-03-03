<template>
  <div id="viewport" class="flex flex-col p-4">
    <div id="threejs-container" class="py-5"></div>
  </div>
</template>

<script setup>
// Imports;
import { onMounted, onUpdated } from "vue";
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
//import image from ".src/assets/Buckminster-Fuller.png"

//<img src="image"/>

// Property coming from parent component
const props = defineProps(["size"]);

// Three js objects
let renderer, camera, scene, controls, geometry, mat;

let width = 700;
let height = 785;

function init() {
  // rendeder
  renderer = new THREE.WebGLRenderer();
  renderer.setSize(width, height);
  renderer.setPixelRatio(window.devicePixelRatio);
  document.getElementById("threejs-container").appendChild(renderer.domElement);

  // camera
  camera = new THREE.PerspectiveCamera(75, width / height, 0.1, 1000);
  camera.position.set(0, 0, 40);

  // scene
  scene = new THREE.Scene();
  scene.background = new THREE.Color("darkgrey");

  // orbit controls
  controls = new OrbitControls(camera, renderer.domElement);

  //directional lights
  const directionalLight1 = new THREE.DirectionalLight( 0xffffff )
  directionalLight1.position.set( 0, 50, 200 )
  directionalLight1.castShadow = true
  directionalLight1.intensity = 10
  scene.add( directionalLight1 )

  const directionalLight2 = new THREE.DirectionalLight( 0xffffff )
  directionalLight2.position.set( 50, 0, 200 )
  directionalLight2.castShadow = true
  directionalLight2.intensity = 10
  scene.add( directionalLight2 )

  const ambientLight = new THREE.AmbientLight( 0xffffff )
  ambientLight.position.set( 50, 50, 200 )
  ambientLight.castShadow = true
  ambientLight.intensity = 10
    scene.add( ambientLight )

  // add fun shape
  createDodechahedron(15, 0);
  animate();
}

// for controls update
function animate() {
  requestAnimationFrame(animate);
  controls.update();
  renderer.render(scene, camera);
}

function createDodechahedron(r,d,) {
  geometry = new THREE.DodecahedronGeometry(r,d);
  const mat = new THREE.MeshNormalMaterial();

  // const loader = new THREE.TextureLoader();
  // const mat = new THREE.MeshBasicMaterial({
  //  color: 0xFF8844,
  //  map: loader.load(image),
  //});
  const sphere = new THREE.Mesh(geometry, mat);
  scene.add(sphere);
  // const mat = new THREE.MeshStandardMaterial({
  //   color:"Magenta",
  //   flatShading: true
  // });
  // const sphere = new THREE.Mesh(geometry, mat);
  // scene.add(sphere);
}

function onSliderChange(color) {
  scene.clear();
  createDodechahedron(props.size, 0);
}

// This function runs at the beginning of the component lifecycle.
// More about Vue lifecycles: https://vuejs.org/guide/essentials/lifecycle.html#lifecycle-diagram
onMounted(() => {
  init();
  animate();
});

// This function runs when DOM updates.
onUpdated(() => {
  // text content should be the same as current `count.value`
  onSliderChange();
});
</script>

<style scoped>
/* #viewport {
  border-style: dashed;
  border-color: #d2dfe8;
  border-width: 4px;
  border-radius: 10px;
  margin: 12px;
  height: calc(100vh - 105px);
  width: 600px;
  min-width: 200px;
  position: inherit;
} */
</style>