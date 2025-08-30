<script setup>
import { RouterLink, RouterView } from 'vue-router'
import CityBackground from './components/CityBackground.vue';
import Fantasy from './components/Fantasy.vue';
import Tesseract from './components/Tesseract.vue';
import Submerged from './components/Submerged.vue';
import Satellites from './components/Satellites.vue';
</script>

<template>
  <Tabs value="0">
    <TabList class="tab">
      <Tab value="0">Distopian City</Tab>
      <Tab value="1">Fantasy Ruins</Tab>
      <Tab value="2">Tesseract</Tab>
      <Tab value="3">Submerged</Tab>
      <Tab value="4">Satellites</Tab>
    </TabList>

    <TabPanels class="tab">

      <TabPanel value="0">
        <CityBackground>
          <main class="text-center mt-32 text-white">
            <h1 class="text-5xl font-bold">Welcome to Distopian City</h1>
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
        </CityBackground>
      </TabPanel>

      <TabPanel value="1">
        <Fantasy>
          <main class="text-center mt-32 text-white">
            <h1 class="text-5xl font-bold">Welcome to Fantasy Ruins</h1>
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

        </Fantasy>
      </TabPanel>

      <TabPanel value="2">
        <Tesseract>
          <main class="text-center mt-32 text-white">
            <h1 class="text-5xl font-bold">Welcome to The Tesseract</h1>
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

        </Tesseract>
      </TabPanel>

      <TabPanel value="3">
        <Submerged>
          <main class="text-center mt-32 text-white">
            <h1 class="text-5xl font-bold">Welcome to Submerged</h1>
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

        </Submerged>
      </TabPanel>

      <TabPanel value="4">
        <Satellites>
          <main class="text-center mt-32 text-white">
            <h1 class="text-5xl font-bold">Welcome to Satellites</h1>
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

        </Satellites>
      </TabPanel>

    </TabPanels>

  </Tabs>
</template>

<style>

.tab {
  background: #18181b85 !important;
}

header {
  line-height: 1.5;
  max-height: 100vh;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 12px;
  text-align: center;
  margin-top: 2rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  }
}
</style>
