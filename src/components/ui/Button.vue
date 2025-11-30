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

const baseClasses =
  [
    // layout
    "inline-flex items-center justify-center gap-2 rounded-full px-6 py-2 text-sm font-semibold",
    // motion
    "transition duration-200 ease-out",
    // focus visible - light and dark
    "focus-visible:outline-none",
    "focus-visible:ring-2 focus-visible:ring-brand-teal focus-visible:ring-offset-2 focus-visible:ring-offset-white",
    "dark:focus-visible:ring-brand-accent dark:focus-visible:ring-offset-slate-950",
    // disabled
    "disabled:opacity-60 cursor-pointer disabled:cursor-not-allowed",
  ].join(" ")

const animationClasses =
  " hover:shadow-lg "

const variantClasses = computed(() => {
  if (variant.value === "secondary") {
    return [
      // light
      "border border-brand-deep text-brand-deep bg-transparent",
      "hover:bg-brand-deep/5 hover:text-brand-deep",
      // dark
      "dark:border-brand-accent dark:text-brand-accent dark:bg-transparent",
      "dark:hover:bg-slate-900 dark:hover:text-brand-chip",
    ].join(" ")
  }

  // primary
  return [
    // light
    "bg-brand-deep text-white shadow-md shadow-cyan-100/80",
    "hover:bg-brand-hover",
    // dark
    "dark:bg-brand-accent dark:text-slate-950 dark:shadow-brand-primary/60",
    "dark:hover:bg-brand-deep dark:hover:text-white",
  ].join(" ")
})

const widthClass = computed(() => (props.fullWidth ? "w-full" : "w-auto"))
</script>

<template>
  <button
    :type="props.type || 'button'"
    :disabled="disabled"
    :class="[baseClasses, animationClasses, variantClasses, widthClass]"
  >
    <slot />
  </button>
</template>
