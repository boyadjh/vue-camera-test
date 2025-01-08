<script setup lang="ts">
import { onMounted } from 'vue';

let video: HTMLVideoElement | null = null;
let canvas: HTMLCanvasElement | null = null;
let context: CanvasRenderingContext2D | null = null;

function photo(): void {
  const dataUrl = canvas?.toDataURL('image/png') + '';
  downloadImage(dataUrl);
}

function downloadImage(data: string, filename = 'untitled.jpeg') {
  const a = document.createElement('a');
  a.href = data;
  a.download = filename;
  document.body.appendChild(a);
  a.click();
}

onMounted(() => {


  canvas = <HTMLCanvasElement>document.getElementById('canvas');
  if (!canvas) {
    throw new Error('canvas not found');
  }

  context = canvas.getContext('2d');

  const promise = navigator.mediaDevices.getUserMedia({ video: true });

  promise.then(res => {

    video = <HTMLVideoElement>document.createElement('video');
    if (!video) {
      throw new Error('video not found');
    }
    video.autoplay = true;
    video.playsInline = true;
    video.srcObject = res;
    video.play();

    video.onloadeddata = () => {
      updateCanvas();
    }
  }).catch(e => console.log(e));

});

function updateCanvas() {
  if (!video || !canvas) {
    throw new Error('video or canvas not found');
  }
  const vRatio: number = (canvas.height / video.videoHeight) * video.videoWidth;
  context?.drawImage(video, 0, 0, vRatio, canvas.height);
  // const imgData: ImageData = context?.getImageData(0, 0, canvas.width, canvas.height);
  // for(let i = 0; i < imgData.data.length; i += 3) {
  //   for (let j = i; j < i+3; j++) {
  //     imgData.data[j] = 255-imgData.data[j];
  //   }
  // }
  // context?.putImageData(imgData, 0, 0);
  window.requestAnimationFrame(updateCanvas);
}
</script>

<template>
  <button @click="photo()">Save Photo</button>
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
}
</style>
