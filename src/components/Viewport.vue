<script setup lang="ts">
import { ref, onMounted } from 'vue'

const viewport = ref<HTMLCanvasElement | null>(null)

onMounted(async () => {
    if (!viewport.value) return

    // Set the size of the drawingBuffer
    viewport.value.width = viewport.value.offsetWidth;
    viewport.value.height = viewport.value.offsetHeight;


    // WebGPU device initialization
    if (!navigator.gpu) {
        throw new Error("WebGPU not supported on this browser.");
    }

    const adapter = await navigator.gpu.requestAdapter();
    if (!adapter) {
        throw new Error("No appropriate GPUAdapter found.");
    }

    const device = await adapter.requestDevice();

    const context = viewport.value.getContext("webgpu");
    const canvasFormat = navigator.gpu.getPreferredCanvasFormat();

    context?.configure({
    device: device,
    format: canvasFormat,
    });

    // Clear the canvas with a render pass
    const encoder = device.createCommandEncoder();

    const pass = encoder.beginRenderPass({
        colorAttachments: [{
                view: context?.getCurrentTexture().createView(),
                loadOp: "clear",
                clearValue: { r: 0, g: 0, b: 0.4, a: 1.0 },
                storeOp: "store",
            }]
    } as GPURenderPassDescriptor);

    pass.end();

    device.queue.submit([encoder.finish()]);

})

</script>

<template>
    <div>
        <canvas ref="viewport" class="viewport"></canvas>
    </div>
</template>

<style scoped>
.viewport {
    width: 100%;
    aspect-ratio: 1.25 / 1;
}
</style>


