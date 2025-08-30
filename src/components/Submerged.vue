<script setup>
import { onMounted } from 'vue'
import * as THREE from 'three'
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js'

onMounted(() => {
  const scene = new THREE.Scene()

  // Camera
  const camera = new THREE.PerspectiveCamera(
    60,
    window.innerWidth / window.innerHeight,
    0.1,
    1000
  )
  camera.position.set(0.07, 0.3, 0)
  camera.rotation.x = THREE.MathUtils.degToRad(-90)



  // Renderer
  const renderer = new THREE.WebGLRenderer({ alpha: true, antialias: true })
  renderer.setSize(window.innerWidth, window.innerHeight)
  renderer.setPixelRatio(window.devicePixelRatio)
  document.getElementById('bg-canvas-4').appendChild(renderer.domElement)

  // Lights
  scene.add(new THREE.AmbientLight(0xffffff, 0.6))
  const dirLight = new THREE.DirectionalLight(0xffffff, 1)
  dirLight.position.set(5, 10, 7)
  scene.add(dirLight)

  // Load background texture
  let skyTexture
  const textureLoader = new THREE.TextureLoader()
  textureLoader.load('/sky.jpg', (texture) => {
    texture.wrapS = THREE.ClampToEdgeWrapping
    texture.wrapT = THREE.ClampToEdgeWrapping

      // Move the texture up by adjusting the Y offset
    texture.offset.y = 50; // Adjust this value to move the texture up

    scene.background = texture
    skyTexture = texture
  })

  // Load the GLB model
  let model
  const loader = new GLTFLoader()
  loader.load('/public/submerged.glb', (gltf) => {
    model = gltf.scene
    scene.add(model)

    // Rotate 30 degrees to the left at start
    model.rotation.y = THREE.MathUtils.degToRad(0)
  })

  // --- Smooth parallax setup ---
  const target = { x: 0, y: 0 }
  const current = { x: 0, y: 0 }
  const baseRotationY = THREE.MathUtils.degToRad(0)
  const baseRotationX = 0

  document.addEventListener('mousemove', (e) => {
    target.x = (e.clientX / window.innerWidth - 0.5) * 3
    target.y = (e.clientY / window.innerHeight - 0.5) * 3
  })

  function animate() {
    requestAnimationFrame(animate)

    // Smoothly interpolate
    current.x += (target.x - current.x) * 0.05
    current.y += (target.y - current.y) * 0.05

    if (model) {
      model.rotation.z = baseRotationY + current.x * 0.2
      model.rotation.x = baseRotationX + current.y * 0.1
    }

    renderer.render(scene, camera)
  }
  animate()

  // Resize handler
  window.addEventListener('resize', () => {
    camera.aspect = window.innerWidth / window.innerHeight
    camera.updateProjectionMatrix()
    renderer.setSize(window.innerWidth, window.innerHeight)
  })
})
</script>


<template>

    <!-- Three.js canvas -->
    <div id="bg-canvas-4"></div>

    <!-- Slot for overlay content -->
    <div class="relative z-10 w-full h-full flex flex-col items-center justify-start">
      <slot />
    </div>

</template>

<style>
/* bg-canvas fills entire screen */
#bg-canvas-4 canvas {
    width: 100%;
    height: 100%;
    display: block;
}

/* Make the canvas stay behind everything */
#bg-canvas-4 {
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
