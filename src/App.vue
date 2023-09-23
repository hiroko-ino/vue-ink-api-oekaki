<script setup lang="ts">
import { ref, onMounted } from 'vue'

declare var navigator:any;

const canvasRef = ref<HTMLCanvasElement | null>(null)
const style = ref({ color: "rgba(0, 255, 0, 1)", diameter: 10 })

onMounted(async () => {
  if (!canvasRef.value) return
  const ctx = canvasRef.value.getContext("2d")
  const presenter = await navigator.ink.requestPresenter({ presentationArea: canvasRef.value });

  console.log(presenter)

  canvasRef.value.addEventListener("pointermove", async (evt) => {
    if (ctx === null) return
    const pointSize = 10;
    ctx.fillStyle = style.value.color;
    ctx.fillRect(evt.pageX, evt.pageY, pointSize, pointSize);
    await presenter.updateInkTrailStartPoint(evt, style.value);
  })

  window.addEventListener("pointerdown", () => {
    if (ctx === null) return
    ctx.clearRect(0, 0, ctx.canvas.width, ctx.canvas.height);
  })
})

</script>

<template>
  <canvas width="800" height="500" id="canvas" ref="canvasRef"></canvas>
</template>

<style scoped>
</style>
