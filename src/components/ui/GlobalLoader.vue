<template>
  <div
    class="fixed inset-0 z-[99999] flex items-center justify-center bg-slate-50/95 dark:bg-slate-950/95"
    role="status"
    aria-busy="true"
  >
    <div class="flex flex-col items-center space-y-4">
      <svg
        viewBox="0 0 300 64"
        class="w-[300px] h-16 text-brand-accent"
        aria-hidden="true"
      >
        <!-- Baseline -->
        <polyline
          points="0,32 120,32 125,20 130,44 135,8 140,56 145,32 300,32"
          fill="none"
          stroke="currentColor"
          stroke-width="3"
          stroke-linecap="round"
          stroke-linejoin="round"
          opacity="0.35"
        />

        <!-- Pulse highlight -->
        <polyline
          class="pulse-highlight"
          points="0,32 120,32 125,20 130,44 135,8 140,56 145,32 300,32"
          fill="none"
          stroke="currentColor"
          stroke-width="5"
          stroke-linecap="round"
          stroke-linejoin="round"
        />
      </svg>

      <p class="text-slate-900 dark:text-slate-50 text-xl font-medium">
        Loading…
        <span class="sr-only">Please wait while we load your content.</span>
      </p>
    </div>
  </div>
</template>

<style scoped>
.pulse-highlight {
  stroke-dasharray: 130 2000;
  stroke-dashoffset: 2130;
  animation: sweep 3.5s linear infinite;
  filter: drop-shadow(0 0 8px rgba(0, 193, 178, 0.9));
}

/* Accessibility */
@media (prefers-reduced-motion: reduce) {
  .pulse-highlight {
    animation: none;
    stroke-dasharray: none;
    stroke-dashoffset: 0;
    opacity: 0.9;
    filter: none;
  }
}

@keyframes sweep {
  0% {
    stroke-dashoffset: 2130;
    opacity: 0.85;
  }
  50% {
    stroke-dashoffset: 1000;
    opacity: 1;
  }
  100% {
    stroke-dashoffset: -130;
    opacity: 0.85;
  }
}
</style>
