<template>
    {{ x }} {{ y }}
    <TresCanvas clear-color="#008080" window-size class="canva">
        <TresPerspectiveCamera
            ref="cameraRef"
        />
        <TresMesh
            ref="boxRef"
            :position="[0, 5, 0]"
        >
            <TresBoxGeometry :args="[1, 1, 1]" />
            <TresMeshNormalMaterial />
        </TresMesh>
        <Text3D
            :position="[0, 0, 0]"
            font="https://raw.githubusercontent.com/Tresjs/assets/main/fonts/FiraCodeRegular.json"
        >
            Hello ewew ewewe
        </Text3D>
        <TresDirectionalLight :position="[0, 2, 4]" :intensity="1.2" cast-shadow />
        <TresMeshMatcapMaterial />
    </TresCanvas>
    <div ref="el" class="container">
        <section class="section">
            <h1>My Portfolio</h1>
        </section>
        <section class="section">
            <h2>My projects</h2>
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
        position: fixed;
        top: 0;
        left: 0;
        width: 100vw;
        height: 100vh;
        overflow-y: scroll;
        scroll-snap-type: y proximity;
    }
    .container::-webkit-scrollbar {
        width: 0px;
        background: transparent;
    }
    
    .container::-webkit-scrollbar-thumb {
        background: transparent;
    }
    .section {
        display: flex;
        scroll-snap-align: center;
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
    }

</style>

<script setup lang="ts">
import { TresCanvas } from '@tresjs/core'
import { Text3D } from '@tresjs/cientos'
import { shallowRef } from 'vue'
import { useScroll } from '@vueuse/core'


const boxRef = shallowRef()
const cameraRef = shallowRef()

const el = ref(null)
const { x, y, isScrolling, arrivedState, directions } = useScroll(el)

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