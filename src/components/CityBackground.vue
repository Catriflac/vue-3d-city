<script setup>
import { onMounted } from 'vue'
import * as THREE from 'three'
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js'

onMounted(() => {
    const scene = new THREE.Scene()

    // Load background texture
    const textureLoader = new THREE.TextureLoader()
    textureLoader.load('/sky.jpg', (texture) => {
        // Enable transforms
        texture.wrapS = THREE.ClampToEdgeWrapping
        texture.wrapT = THREE.ClampToEdgeWrapping

        // Stretch (zoom in 20% → use 0.8 scale)
        texture.repeat.set(0.8, 0.8)
        texture.center.set(0.5, 0.5) // keep scaling centered

        scene.background = texture

        document.addEventListener('mousemove', (e) => {
            const x = (e.clientX / window.innerWidth - 0.5) * 2
            const y = (e.clientY / window.innerHeight - 0.5) * 2

            // Offset inside the stretched texture (opposite movement)
            texture.offset.x = -x * 0.035
            texture.offset.y = -y * 0.1

            // on x movenent, rotate the texture a bit
            texture.rotation = -x * 0.1
        })
    })


    // Camera
    const camera = new THREE.PerspectiveCamera(
        60,
        window.innerWidth / window.innerHeight,
        0.1,
        1000
    )
    camera.position.set(0, 5, 10)

    // Renderer
    const renderer = new THREE.WebGLRenderer({ alpha: true, antialias: true })
    renderer.setSize(window.innerWidth, window.innerHeight)
    renderer.setPixelRatio(window.devicePixelRatio)
    document.getElementById('bg-canvas').appendChild(renderer.domElement)

    // Lights
    scene.add(new THREE.AmbientLight(0xffffff, 0.6))
    const dirLight = new THREE.DirectionalLight(0xffffff, 1)
    dirLight.position.set(5, 10, 7)
    scene.add(dirLight)

    // Load the GLB
    const loader = new GLTFLoader()
    loader.load('/public/city.glb', (gltf) => {
        const model = gltf.scene
        scene.add(model)

        // Rotate 30 degrees to the left at start
        model.rotation.y = THREE.MathUtils.degToRad(-20)
        // Rotate camera up a bit
        camera.rotation.x = THREE.MathUtils.degToRad(30)

        // Move camera back a bit
        camera.position.set(0, 0, 60)
        // Move the camera down a bit
        camera.position.y = -25

        const baseRotationY = THREE.MathUtils.degToRad(-30)
        const baseRotationX = 0

        // Mouse parallax
        document.addEventListener('mousemove', (e) => {
            const x = (e.clientX / window.innerWidth - 0.5) * 2
            const y = (e.clientY / window.innerHeight - 0.5) * 2
            // model.rotation.y = x * 0.4
            // model.rotation.x = y * 0.2
            // Apply relative to base
            model.rotation.y = baseRotationY + x * 0.2
            model.rotation.x = baseRotationX + y * 0.1
        })

        animate()
    })

    function animate() {
        requestAnimationFrame(animate)
        renderer.render(scene, camera)
    }

    // Resize handler
    window.addEventListener('resize', () => {
        camera.aspect = window.innerWidth / window.innerHeight
        camera.updateProjectionMatrix()
        renderer.setSize(window.innerWidth, window.innerHeight)
    })
})
</script>

<template>
    <!-- <div id="bg-canvas"></div> -->


    <!-- Three.js canvas -->
    <div id="bg-canvas"></div>


    <!-- Overlay content -->
    <div class="relative z-10">
        <main class="text-center mt-32 text-white">

            <h1 class="text-5xl font-bold">Welcome to My 3D City</h1>
            <p class="mt-4 text-xl">This is a Three.js background with interactive parallax.</p>

            <div class="mt-24">
                <p>Built using </p>
                <ul class="mt-4 text-xl">
                    <li>Vue.js for the frontend framework</li>
                    <li>Three.js for 3D graphics rendering</li>
                    <li>GLTFLoader for loading 3D models in GLB format</li>
                </ul>
                <p>Experience smooth animations and responsive design with modern web technologies</p>
            </div>


        </main>
    </div>
</template>

<style>
/* bg-canvas fills entire screen */
#bg-canvas canvas {
    width: 100%;
    height: 100%;
    display: block;
}

/* Make the canvas stay behind everything */
#bg-canvas {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -10;
    /* behind everything */
}

/* Overlay container */
.relative {
    position: relative;
    z-index: 10;
    /* above the canvas */
}
</style>
