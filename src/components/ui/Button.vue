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
  "motion-safe:transition-all motion-safe:duration-200 motion-safe:ease-out",
  "motion-safe:hover:shadow-lg motion-safe:active:shadow-md",
  "motion-reduce:transition-none motion-reduce:hover:shadow-md motion-reduce:active:shadow-sm",
].join(" ")

const variantClasses = computed(() => {
  if (variant.value === "secondary") {
    return [
      // base colors
      "bg-transparent border border-brand-deep/80 text-brand-deep",
      "dark:bg-transparent dark:border-brand-accent/80 dark:text-brand-accent",

      // hover text color only when motion is allowed
      "motion-safe:hover:text-white",
      "motion-safe:group-hover:text-white",

      // on reduce-motion text stays the same in both themes
      "motion-reduce:hover:text-brand-deep",
      "motion-reduce:dark:hover:text-brand-accent",

      // ring behavior
      "hover:ring-2 hover:ring-offset-2 hover:ring-brand-teal",
      "dark:hover:ring-2 dark:hover:ring-offset-2 dark:hover:ring-brand-accent dark:hover:ring-offset-black",

      // subtle shadow
      "shadow-md shadow-cyan-100/40 dark:shadow-brand-primary/40",
    ].join(" ")
  }

  // primary button
  return [
    // base colors
    "bg-brand-deep text-white shadow-md shadow-cyan-100/80",
    "dark:bg-brand-deep dark:text-white dark:shadow-brand-primary/60",

    // ring behavior
    "hover:ring-2 hover:ring-offset-2 hover:ring-brand-teal",
    "dark:hover:ring-2 dark:hover:ring-offset-2 dark:hover:ring-brand-accent dark:hover:ring-offset-black",

    // only animate text color when motion is allowed
    "motion-safe:group-hover:text-white",

    // no motion-reduce text classes here
    // so on prefers-reduced-motion the text stays exactly white in both themes
  ].join(" ")
})

const overlayClasses = computed(() => {
  if (variant.value === "secondary") {
    return "bg-brand-deep dark:bg-brand-accent origin-right"
  }

  return "bg-brand-hover dark:bg-brand-accent origin-left"
})

const widthClass = computed(() => (props.fullWidth ? "w-full" : "w-auto"))
</script>

<template>
  <button
    :type="props.type || 'button'"
    :disabled="props.disabled"
    :class="[baseClasses, motionClasses, variantClasses, widthClass]"
  >
    <!-- background sweep -->
    <span
      aria-hidden="true"
      :class="[
        'pointer-events-none absolute inset-0 rounded-full z-0',
        'scale-x-0',
        'motion-safe:transition-transform motion-safe:duration-200 motion-safe:ease-out',
        'motion-safe:group-hover:scale-x-100',
        'motion-reduce:hidden',
        overlayClasses,
      ]"
    />

    <span class="relative z-10 flex items-center gap-2">
      <slot />
    </span>
  </button>
</template>
