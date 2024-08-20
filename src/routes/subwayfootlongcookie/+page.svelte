<script lang="ts">
  import { Canvas } from "@threlte/core";
  import { T } from "@threlte/core";
  import * as THREE from "three";
  import { OrbitControls, Environment, Sky } from "@threlte/extras";
  import { onMount } from "svelte";
  import Lenis from "@studio-freight/lenis";
  import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader.js";
  import { useLoader } from "@threlte/core";
  import Cookie from "./cookie.svelte";

  //const gltf = useLoader(GLTFLoader).load("cookie.glb");
  let scrollY = 0; // Store the scroll position

  let lenis;

  onMount(() => {
    const handleScroll = () => {
      scrollY = window.scrollY; // Capture the vertical scroll position
    };

    // Attach Lenis scroll event
    lenis = new Lenis({
      smooth: true,
      direction: "vertical", // or 'horizontal'
      mouseMultiplier: 1.2, // Adjust scrolling speed
    });

    lenis.on("scroll", handleScroll);

    function raf(time) {
      lenis.raf(time);
      requestAnimationFrame(raf);
    }

    requestAnimationFrame(raf);

    return () => {
      lenis.destroy(); // Clean up when the component unmounts
    };
  });
</script>

<section class="p-5">
  <div
    class="canvas-wrapper"
    style="height:100%; position:fixed; width:100%;  !important; z-index:2
  "
  >
    <Canvas>
      <Cookie
        position.x={0}
        scale={0.25}
        rotation={[
          0.5 - scrollY * 0.00005,
          0.7 + scrollY * 0.013,
          scrollY * 0.0005,
        ]}
        z-index:2
      />
    </Canvas>
  </div>
</section>
<div style="height:25vh"></div>

<h1 style="color:white; text-align:center; font-size: 8vh">
  Subway Footlong Cookie
</h1>

<div style="height:100vh"></div>
<h1 style="color:white; text-align:center">"I've had longer" -Frank</h1>
<div style="height:30vh"></div>

<style>
  :global(body) {
    /* this will apply to <body> */
    margin: 0;
    padding: 0;
    background-color: black;
  }
  html,
  #app {
    margin: 0;
    padding: 0;
    width: 100%;
    height: 100%;
    overflow: hidden; /* Ensures no scrollbar appears */
  }

  canvas {
    height: 1000px !important;
    display: block; /* Removes gaps caused by inline canvas elements */
    z-index: -1;
  }

  html.lenis,
  html.lenis {
    height: auto;
  }
  #canvas-wrapper {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -2; /* Make sure the canvas is behind other content */
    pointer-events: none; /* Optional: This prevents the canvas from interfering with pointer events (like clicks) */
  }

  .lenis.lenis-smooth {
    scroll-behavior: auto !important;
  }

  .lenis.lenis-smooth [data-lenis-prevent] {
    overscroll-behavior: contain;
  }

  .lenis.lenis-stopped {
    overflow: hidden;
  }

  .lenis.lenis-smooth iframe {
    pointer-events: none;
  }
</style>
