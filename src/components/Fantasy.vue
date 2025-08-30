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
  camera.position.set(-2, 0, 10)
  camera.rotation.x = THREE.MathUtils.degToRad(0)
  camera.rotation.y = THREE.MathUtils.degToRad(0)

  // Renderer
  const renderer = new THREE.WebGLRenderer({ alpha: true, antialias: true })
  renderer.setSize(window.innerWidth, window.innerHeight)
  renderer.setPixelRatio(window.devicePixelRatio)
  document.getElementById('bg-canvas-2').appendChild(renderer.domElement)

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
    texture.repeat.set(0.8, 0.8)
    texture.center.set(0.5, 0.5)
    scene.background = texture
    skyTexture = texture
  })

  // Load the GLB model
  let model
  const loader = new GLTFLoader()
  loader.load('/fantasy.glb', (gltf) => {
    model = gltf.scene
    scene.add(model)

    // Rotate 30 degrees to the left at start
    model.rotation.y = THREE.MathUtils.degToRad(180)
  })

  // --- Smooth parallax setup ---
  const target = { x: 0, y: 0 }
  const current = { x: 0, y: 0 }
  const baseRotationX = 0
  const baseRotationY = THREE.MathUtils.degToRad(-180)

  document.addEventListener('mousemove', (e) => {
    target.x = (e.clientX / window.innerWidth - 0.5) * 2
    target.y = (e.clientY / window.innerHeight - 0.5) * 2
  })

  function animate() {
    requestAnimationFrame(animate)

    // Smoothly interpolate
    current.x += (target.x - current.x) * 0.05
    current.y += (target.y - current.y) * 0.05

    if (model) {
      model.rotation.y = baseRotationY + current.x * 0.2
      model.rotation.x = baseRotationX + current.y * 0.1
    }

    if (skyTexture) {
      skyTexture.offset.y = -current.y * 0.1
      skyTexture.rotation = -current.x * -0.1
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
    <div id="bg-canvas-2"></div>

    <!-- Slot for overlay content -->
    <div class="relative z-10 w-full h-full flex flex-col items-center justify-start">
      <slot />
    </div>

</template>

<style>
/* bg-canvas fills entire screen */
#bg-canvas-2 canvas {
    width: 100%;
    height: 100%;
    display: block;
}

/* Make the canvas stay behind everything */
#bg-canvas-2 {
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
