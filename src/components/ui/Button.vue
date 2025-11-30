<script setup lang="ts">
import { computed } from "vue"

type Variant = "primary" | "secondary"

const props = defineProps<{
  variant?: Variant
  type?: "button" | "submit" | "reset"
  disabled?: boolean
  fullWidth?: boolean
}>()

const variant = computed<Variant>(() => props.variant || "primary")

const baseClasses = [
  "relative inline-flex items-center justify-center gap-2 rounded-full px-6 py-2 text-sm font-semibold",
  "group overflow-hidden isolate",
  "focus-visible:outline-none",
  "focus-visible:ring-2 focus-visible:ring-brand-teal focus-visible:ring-offset-2 focus-visible:ring-offset-white",
  "dark:focus-visible:ring-brand-accent dark:focus-visible:ring-offset-slate-950",
  "cursor-pointer disabled:opacity-60 disabled:cursor-not-allowed",
].join(" ")

const motionClasses = [
  // keep animation but do not transform the button itself to avoid text jank
  "motion-safe:transition-shadow motion-safe:duration-200 motion-safe:ease-out",
  "motion-safe:hover:shadow-lg motion-safe:active:shadow-md",
  "motion-reduce:transition-none motion-reduce:hover:shadow-md motion-reduce:active:shadow-sm",
].join(" ")

const variantClasses = computed(() => {
  if (variant.value === "secondary") {
    return [
      "bg-transparent text-brand-deep border border-brand-deep/80",
      "dark:bg-transparent dark:text-brand-accent dark:border-brand-accent/80",
      // reduced motion fallback - simple subtle bg change
      "motion-reduce:hover:bg-brand-deep/[0.06]",
      "motion-reduce:dark:hover:bg-slate-900/80",
    ].join(" ")
  }

  // primary
  return [
    "bg-brand-deep text-white shadow-md shadow-cyan-100/80 hover:ring-2 hover:ring-offset-2 hover:ring-brand-accent ",
    "dark:bg-brand-deep dark:hover:ring-offset-black dark:text-white dark:shadow-brand-primary/60 dark:hover:text-slate-950",
    // reduced motion - simple color change only
    "motion-reduce:hover:bg-brand-hover ",
    "motion-reduce:dark:hover:bg-brand-deep motion-reduce:dark:hover:text-white ",
  ].join(" ")
})

const overlayClasses = computed(() => {
  if (variant.value === "secondary") {
    return "bg-brand-deep/10 dark:bg-slate-900/90"
  }

  return "bg-brand-accent  dark:bg-brand-accent"
})

const widthClass = computed(() => (props.fullWidth ? "w-full" : "w-auto"))
</script>

<template>
  <button
    :type="props.type || 'button'"
    :disabled="disabled"
    :class="[baseClasses, motionClasses, variantClasses, widthClass]"
  >
    <!-- left to right solid color fill under the text -->
    <span
      aria-hidden="true"
      :class="[
        'pointer-events-none absolute inset-0 rounded-full z-0',
        'origin-left scale-x-0',
        'motion-safe:transition-transform motion-safe:duration-200 motion-safe:ease-out',
        'motion-safe:group-hover:scale-x-100',
        'motion-reduce:hidden',
        overlayClasses,
      ]"
    />

    <!-- content stays on its own stable layer -->
    <span class="relative z-10 flex items-center gap-2">
      <slot />
    </span>
  </button>
</template>
