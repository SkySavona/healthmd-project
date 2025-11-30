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
  "inline-flex items-center justify-center gap-2 rounded-full px-6 py-2 text-sm font-semibold " +
  "transition duration-200 ease-out " +
  "focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-brand-teal focus-visible:ring-offset-2 focus-visible:ring-offset-white " +
  "disabled:opacity-60 cursor-pointer disabled:cursor-not-allowed"

const animationClasses =
  "hover:-translate-y-0.5 hover:shadow-lg active:translate-y-0 active:scale-[0.99]"

const variantClasses = computed(() => {
  if (variant.value === "secondary") {
    return (
      "border border-brand-deep text-brand-deep bg-transparent " +
      "hover:bg-brand-deep/5"
    )
  }

  // primary
  return (
    "bg-brand-deep text-white shadow-md shadow-cyan-100/80 " +
    "hover:bg-brand-hover"
  )
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
