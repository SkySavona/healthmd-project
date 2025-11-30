<script setup lang="ts">
import { ref, onMounted } from "vue"
import HeroSection from "./components/sections/HeroSection.vue"
import GlobalLoader from "./components/ui/GlobalLoader.vue"

const isLoading = ref(true)

onMounted(() => {
  // Fake delay — replace with actual boot logic (router ready, API preload, etc.)
  setTimeout(() => {
    isLoading.value = false
  }, 1200)
})
</script>

<template>
  <div class="min-h-screen">
    <!-- Show loader over entire app -->
    <GlobalLoader v-if="isLoading" />

    <!-- Your real content -->
    <HeroSection v-else />
  </div>
</template>
