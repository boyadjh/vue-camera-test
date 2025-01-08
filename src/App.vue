<script setup lang="ts">
import CameraCapture from '@/components/CameraCapture.vue'
import SnapshotViewer from '@/components/SnapshotViewer.vue'
import { ref } from 'vue'

const snapshots = ref<string[]>([])

const snapshotTaken = (snapshot: string) => {
  snapshots.value.push(snapshot)
}

const deleteSnapshot = (index: number): void => {
  snapshots.value.splice(index, 1)
}
</script>

<template>
  <div class="absolute position-0">
    <header class="ml-1">vue-camera-test</header>
    <div class="flex w-full">
      <camera-capture @snapshot="snapshotTaken" />
    </div>
    <div class="relative bottom-0 left-0 right-0">
      <snapshot-viewer :snapshots="snapshots" @delete="deleteSnapshot" />
    </div>
  </div>
</template>

<style scoped></style>
