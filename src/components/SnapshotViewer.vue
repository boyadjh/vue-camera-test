<script setup lang="ts">
import { defineEmits, defineProps } from 'vue'

const props = defineProps<{
  snapshots: string[]
}>()

const emit = defineEmits<{
  (e: 'delete', index: number): void
}>()

const deleteSnapshot = (index: number): void => {
  emit('delete', index)
}
</script>

<template>
  <div class="w-full bg-neutral-600">
    <p class="text-white" v-html="props.snapshots.length"></p>
    <div class="flex flex-wrap gap-1">
      <div class="relative" v-for="(snapshot, index) in props.snapshots" :key="index">
        <img class="snapshot" :src="snapshot" />
        <span class="delete" @click="deleteSnapshot(index)">X</span>
      </div>
    </div>
  </div>
</template>

<style>
.snapshot {
  @apply h-20 w-auto;
}
.delete {
  @apply absolute top-0 left-0 w-8 h-8 cursor-pointer bg-red-900 bg-opacity-90 text-center;
}
</style>
