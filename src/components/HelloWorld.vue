<script lang="ts">
import Vue from "vue";
// eslint-disable-next-line @typescript-eslint/ban-ts-comment
//@ts-ignore
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader.js";

export default Vue.extend({
  name: "HelloWorld",

  data: () => {
    const scene = new THREE.Scene();
    const camera = new THREE.PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.1,
      1000
    );
    const renderer = new THREE.WebGLRenderer();
    const controls = new OrbitControls(camera, renderer.domElement);

    controls.enableDamping = true;
    camera.position.z = 5;
    camera.position.y = 5;
    camera.position.x = 3;
    controls.minDistance = 2;

    return {
      scene,
      camera,
      renderer,
      controls,
    };
  },

  mounted() {
    this.init();
    this.animate();
  },

  methods: {
    init() {
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      this.renderer.setClearColor("#613CB0");
      this.camera.position.z = 5;
      (this.$refs.canvas as any).appendChild(this.renderer.domElement);
      // this.addModel();
      this.createCube();
    },
    addModel() {
      const loader = new GLTFLoader();
      setTimeout(() => {
        loader.load(
          "../../public/nike/scene",
          (gltf) => {
            console.log(gltf);
            // this.scene.add(gltf.scene);
          },
          (progress) => {
            console.log(progress);
          }
        );
      }, 0);
    },
    createCube() {
      const geometry = new THREE.BoxGeometry(1, 1, 5);
      const edges = new THREE.EdgesGeometry(geometry);
      const line = new THREE.LineSegments(
        edges,
        new THREE.LineBasicMaterial({
          color: "#000",
        })
      );
      const material = new THREE.LineBasicMaterial({
        color: "#FFCB6B",
      });
      const cube = new THREE.Mesh(geometry, material);
      this.scene.add(cube, line);
    },
    animate() {
      requestAnimationFrame(this.animate);
      this.controls.update();
      this.renderer.render(this.scene, this.camera);
    },
  },
});
</script>

<template>
  <div ref="canvas" class="canvas"></div>
</template>

<style scoped>
.canvas {
  width: 100vw;
  height: 100vh;
}
</style>
