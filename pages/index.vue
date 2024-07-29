<template>
    <div ref="el" class="container">
        <TresCanvas clear-color="#008080" window-size class="canva">
            <TresPerspectiveCamera
                ref="cameraRef"
            />
            <!-- <OrbitControls 
            /> -->
            <TresMesh
                ref="boxRef"
                :position="[2, 2, 0]"
            >
                <TresBoxGeometry :args="[1, 1, 1]" />
                <TresMeshNormalMaterial />
            </TresMesh>
            <!-- <TresGridHelper /> -->
        </TresCanvas>
        <section class="section">
            <h1>My Portfolio</h1>
        </section>
        <section class="section">
            <h2>My projects</h2>
            {{ x }} {{ y }} {{ isScrolling }} {{ arrivedState }} {{ directions }}
        </section>
        <section class="section">
            <h2>Contact me</h2>
        </section>
    </div>

</template>

<style scoped>
    .canva {
        z-index: -100;
        position: fixed;
        top: 0;
        left: 0;
    }

    .container {
        color: white;
        height: 100vh;
        width: 100%;
        overflow: scroll;
        overflow-x: hidden;
        scrollbar-width: none;
        scroll-snap-type: y mandatory;
        position: relative;
    }

    .section {
        display: flex;
        align-items: center;
        height: 100vh;
        position: relative;
        font-family: 'Cabin', sans-serif;
        color: #ffeded;
        text-transform: uppercase;
        font-size: 7vmin;
        padding-left: 10%;
        padding-right: 10%;
        will-change: transform;
        overflow-y: hidden;
    }

    section:nth-child(odd) {
        justify-content: flex-end;
    }
</style>

<script setup lang="ts">
import { TresCanvas } from '@tresjs/core'
import { OrbitControls } from '@tresjs/cientos'
import { shallowRef } from 'vue'
import { useScroll } from '@vueuse/core'


const boxRef = shallowRef()
const cameraRef = shallowRef()

const el = ref(null)
const { x, y, isScrolling, arrivedState, directions } = useScroll(el)

let scroll = 0

let radius = 5
let angle = 0

onMounted(() => {
    setInterval(() => {
        angle += 0.01
        cameraRef.value.position.x = boxRef.value.position.x + Math.sin(angle) * radius
        cameraRef.value.position.z = boxRef.value.position.z +  Math.cos(angle) * radius
        cameraRef.value.position.y = boxRef.value.position.y - y.value / 100
        cameraRef.value.lookAt(boxRef.value.position)
        
    }, 1000 / 60)
})

onMounted(() => {
    setInterval(() => {
        boxRef.value.rotation.x += 0.01
        boxRef.value.rotation.y += 0.01

    }, 1000 / 60)
})


</script>