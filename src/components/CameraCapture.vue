<script setup lang="ts">
import { defineEmits, onMounted } from 'vue'

const emit = defineEmits<{
  (e: 'snapshot', uri: string): void
}>()

let canvas: HTMLCanvasElement | null = null
let ctx: CanvasRenderingContext2D | null = null
let video: HTMLVideoElement | null = null

onMounted(() => {
  canvas = <HTMLCanvasElement>document.getElementById('cameraView')
  if (!canvas) {
    throw new Error('Error getting canvas')
  }
  ctx = canvas.getContext('2d')

  const promise = navigator.mediaDevices.getUserMedia({
    video: {
      width: { ideal: 1080 },
      height: { ideal: 1920 },
      frameRate: { ideal: 60 },
    },
  })
  promise
    .then((res) => {
      video = <HTMLVideoElement>document.createElement('video')
      if (!video || !canvas) {
        throw new Error('Error creating video element')
      }
      video.srcObject = res
      video.play()

      const settings = res.getVideoTracks()[0].getSettings()
      canvas.width = settings.width ?? 1920
      canvas.height = settings.height ?? 1080

      video.onloadeddata = () => {
        updateCanvas()
      }
    })
    .catch((e) => console.log(e))
})

const updateCanvas = () => {
  if (!video || !canvas) {
    throw new Error('Error updating canvas')
  }
  const vRatio: number = (canvas.height / video.videoHeight) * video.videoWidth
  ctx?.drawImage(video, 0, 0, vRatio, canvas.height)
  window.requestAnimationFrame(updateCanvas)
}

const snap = () => {
  if (!canvas) {
    throw new Error('Canvas element not found')
  }
  const capturedImage = canvas.toDataURL('image/png')
  emit('snapshot', capturedImage)
}
</script>

<template>
  <div id="cameraVue" class="w-full flex flex-col">
    <button @click="snap()" class="btn ml-2">Capture</button>
    <canvas id="cameraView"></canvas>
  </div>
</template>

<style scoped></style>
