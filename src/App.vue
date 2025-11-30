<script setup>
// import HeroSection from './components/HeroSection.vue'
</script>

<template>
  <div class="app-shell">
    <main class="page">
      <!-- <HeroSection /> -->
      <!-- later we can add HowItWorksSection and IntakeSection -->
    </main>
  </div>
</template>

<style scoped>
.app-shell {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: stretch;
  background: radial-gradient(circle at top left, #0f172a 0, #020617 40%, #020617 100%);
  color: #f9fafb;
}

.page {
  width: 100%;
  max-width: 1120px;
  padding: 3rem 1.5rem;
}
</style>
