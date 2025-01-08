<script setup lang="ts">
import { onMounted } from 'vue';

let video: HtmlVideoElement | null = null;
let canvas: HTMLCanvasElement | null = null;
let context: CanvasRenderingContext2D | null = null;

onMounted(() => {

  canvas = document.getElementById('canvas');
  context = canvas.getContext('2d');

  const promise = navigator.mediaDevices.getUserMedia({ video: true });
  promise.then(res => {
    video = document.createElement('video');
    video.srcObject = res;
    video.play();

    video.onloadeddata = () => {
      updateCanvas();
    }
  }).catch(e => console.log(e));

});

function updateCanvas() {
  context?.drawImage(video, 0, 0);
  window.requestAnimationFrame(updateCanvas);
}
</script>

<template>
  <canvas id="canvas"></canvas>
</template>

<style>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}

#canvas {
  width: 300px;
  aspect-ratio: 16/9;
}
</style>
