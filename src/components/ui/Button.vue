<script setup lang="ts">
import { computed } from "vue"

type Variant = "primary" | "secondary"

const props = defineProps<{
  variant?: Variant
  type?: "button" | "submit" | "reset"
  disabled?: boolean
  fullWidth?: boolean
}>()

const variant = computed(() => props.variant || "primary")

// Disabled appearance ONLY — kills animations, hover, and rings
const disabledClasses = [
  "disabled:bg-gray-300 disabled:text-gray-500 disabled:border-gray-400",
  "dark:disabled:bg-gray-700 dark:disabled:text-gray-400 dark:disabled:border-gray-600",
  "disabled:shadow-none",
  "disabled:hover:none disabled:active:none",
  "disabled:transition-none",
  "disabled:scale-100"
].join(" ")

const baseClasses = [
  "relative inline-flex items-center justify-center gap-2 rounded-full px-6 py-2 text-sm font-semibold",
  "group overflow-hidden isolate select-none",
  "focus-visible:outline-none",
  "focus-visible:ring-2 focus-visible:ring-brand-teal focus-visible:ring-offset-2 focus-visible:ring-offset-white",
  "dark:focus-visible:ring-brand-accent dark:focus-visible:ring-offset-slate-950",
  "cursor-pointer disabled:cursor-not-allowed",
  disabledClasses,
].join(" ")

// Only runs when NOT disabled
const motionClasses = [
  "motion-safe:transition-all motion-safe:duration-200 motion-safe:ease-out",
  "motion-safe:hover:shadow-lg motion-safe:active:shadow-md",
  "motion-reduce:transition-none motion-reduce:hover:shadow-md motion-reduce:active:shadow-sm",
].join(" ")

const variantClasses = computed(() => {
  if (variant.value === "secondary") {
    return [
      "bg-transparent border border-brand-deep/80 text-brand-deep",
      "dark:bg-transparent dark:border-brand-accent/80 dark:text-brand-accent",
      "motion-safe:hover:text-white dark:motion-safe:hover:text-brand-primary",
      "hover:ring-2 hover:ring-offset-2 hover:ring-brand-teal",
      "dark:hover:ring-2 dark:hover:ring-offset-2 dark:hover:ring-brand-accent dark:hover:ring-offset-black",
      "shadow-md shadow-cyan-100/40 dark:shadow-brand-primary/40",
    ].join(" ")
  }

  return [
    "bg-brand-deep text-white shadow-md shadow-cyan-100/80",
    "dark:bg-brand-deep dark:text-white dark:shadow-brand-primary/60",
    "hover:ring-2 hover:ring-offset-2 hover:ring-brand-teal",
    "dark:hover:ring-2 dark:hover:ring-offset-2 dark:hover:ring-brand-accent dark:hover:ring-offset-black",
  ].join(" ")
})

const overlayClasses = computed(() =>
  variant.value === "secondary"
    ? "bg-brand-deep dark:bg-brand-accent origin-right"
    : "bg-brand-hover dark:bg-brand-accent origin-left"
)

const widthClass = computed(() => (props.fullWidth ? "w-full" : "w-auto"))
</script>

<template>
  <button
    :type="props.type || 'button'"
    :disabled="props.disabled"
    :class="[baseClasses, !props.disabled && motionClasses, variantClasses, widthClass]"
  >
    <span
      aria-hidden="true"
      :class="[
        'pointer-events-none absolute inset-0 rounded-full z-0',
        'scale-x-0',
        !props.disabled && 'motion-safe:transition-transform motion-safe:duration-200 motion-safe:ease-out motion-safe:group-hover:scale-x-100',
        'motion-reduce:hidden',
        overlayClasses,
      ]"
    />
    <span class="relative z-10 flex items-center gap-2">
      <slot />
    </span>
  </button>
</template>
