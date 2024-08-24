<script lang="ts">
  //https://whatamesh.vercel.app/ use this thing to make background
  import { Canvas } from "@threlte/core";
  import { T } from "@threlte/core";
  import * as THREE from "three";
  import { OrbitControls, Environment, Sky } from "@threlte/extras";
  import { onMount } from "svelte";
  import Lenis from "@studio-freight/lenis";
  import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader.js";
  import { useLoader } from "@threlte/core";
  import Cookie from "./cookie.svelte";

  import { Gradient } from "./Gradient.ts";

  // Create your instance
  const gradient = new Gradient();

  // Call `initGradient` with the selector to your canvas

  //const gltf = useLoader(GLTFLoader).load("cookie.glb");
  let scrollY = 0; // Store the scroll position
  let cookieRotationX = 0.5;
  let cookieRotationY = 0.7;
  let cookieRotationZ = 0;
  let lenis;

  onMount(() => {
    gradient.initGradient("#gradient-canvas");

    const handleScroll = () => {
      scrollY = window.scrollY; // Capture the vertical scroll position
      let windowHeight = window.innerHeight;
      let height = document.documentElement.scrollHeight - windowHeight;

      let percent = scrollY / height;

      cookieRotationX = 0.5 + percent * (0.45494 - 0.5);
      cookieRotationY = 0.7 + percent * 8.7;
      cookieRotationZ = percent * 0.3;

      // console.log(0.5 - scrollY * 0.0000751);
      // console.log(0.7 + scrollY * 0.0145);
      // console.log(scrollY * 0.0005);
      // console.log(percent + " test");
      // // 0.45494;
      // // 9.4;
      // // 0.3;
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

<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link
  href="https://fonts.googleapis.com/css2?family=Work+Sans:ital,wght@0,100..900;1,100..900&display=swap"
  rel="stylesheet"
/>
<link
  href="https://fonts.googleapis.com/css2?family=Merriweather:ital,wght@0,300;0,400;0,700;0,900;1,300;1,400;1,700;1,900&display=swap"
  rel="stylesheet"
/>
<section class="p-5">
  <div
    class="canvas-wrapper"
    style="height:100%; position:fixed; top:0;width:100%;  !important; z-index:2
  "
  >
    <Canvas>
      <Cookie
        position.x={0}
        scale={0.25}
        rotation={[cookieRotationX, cookieRotationY, cookieRotationZ]}
        z-index:2
      />
    </Canvas>
  </div>
</section>
<canvas id="gradient-canvas" data-transition-in />

<div style="margin-top:31vh"></div>

<h1 class="title" style="color:white; text-align:center; font-size: 8vh">
  Subway Footlong Cookie
</h1>

<div style="margin-top:100vh"></div>
<h1 class="review" style="color:#E6E6E6; text-align:center">
  "I've had longer" -Frank
</h1>
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
    padding: 0;
    width: 100%;
    height: 100%;
    overflow: hidden; /* Ensures no scrollbar appears */
  }

  canvas {
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

  #gradient-canvas {
    position: fixed;
    top: 0;
    width: 100%;
    height: 100%;
    --gradient-color-1: #0f0f0f;
    --gradient-color-2: #000000;
    --gradient-color-3: #0e0e0e;
    --gradient-color-4: #252525;
  }
  .title {
    font-family: "Work Sans", sans-serif;
    font-optical-sizing: auto;
    font-weight: 600;
    font-style: normal;
  }
  .review {
    font-family: "Merriweather", serif;
    font-weight: 400;
    font-style: normal;
  }
</style>
