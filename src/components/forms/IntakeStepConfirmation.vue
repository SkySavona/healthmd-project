<script setup lang="ts">
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome"
import { faCircleCheck } from "@fortawesome/free-solid-svg-icons"
import Button from "@/components/ui/Button.vue"

type Contact = {
  name?: string
  email?: string
  phone?: string
}

const props = defineProps<{
  contact?: Contact
}>()

const emit = defineEmits<{
  (e: "startOver"): void
}>()
</script>

<template>
  <div
    class="flex flex-col items-center justify-center gap-6 py-10 text-center"
    role="status"
    aria-live="polite"
  >
    <div
      class="flex h-20 w-20 items-center justify-center rounded-full bg-brand-chip text-brand-primary shadow-md dark:bg-brand-primary dark:text-brand-chip dark:shadow-brand-primary/60 transition duration-300"
      aria-hidden="true"
    >
      <FontAwesomeIcon :icon="faCircleCheck" class="text-3xl" />
    </div>

    <div class="space-y-2 max-w-md">
      <h2 class="text-2xl font-semibold text-slate-900 dark:text-slate-50">
        Intake submitted successfully
      </h2>

      <p class="text-sm text-slate-600 dark:text-slate-300">
        Thank you
        <span class="font-medium" v-if="contact?.name">
          {{ contact.name }}
        </span>
        — our clinical team will review your details and follow up shortly.
      </p>
    </div>

    <div class="text-sm text-slate-500 dark:text-slate-400 max-w-sm leading-relaxed">
      We will contact you at
      <span class="font-medium text-brand-primary dark:text-brand-accent">
        {{ contact?.email || "your email" }}
      </span>
      and
      <span class="font-medium text-brand-primary dark:text-brand-accent">
        {{ contact?.phone || "your phone number" }}
      </span>
      if we need anything before your visit begins.
    </div>

    <div class="mt-6 flex flex-wrap justify-center gap-3">

      <Button
        type="button"
        variant="primary"
        @click="emit('startOver')"
      >
        New intake
      </Button>

    
    </div>

    <p class="mt-4 text-sm  text-slate-500 dark:text-slate-500 max-w-xs">
      If something isn’t correct, let your clinician know when your appointment begins.
    </p>
  </div>
</template>
