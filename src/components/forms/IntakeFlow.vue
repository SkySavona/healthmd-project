<script setup lang="ts">
import { reactive, ref, computed } from "vue"
import IntakeStepReason from "@/components/forms/IntakeStepReason.vue"
import IntakeStepHistory from "@/components/forms/IntakeStepHistory.vue"
import IntakeStepDetails from "@/components/forms/IntakeStepDetails.vue"

const currentStep = ref(1)
const totalSteps = 3

const formData = reactive({
  reason: "weight-long-term",
  history: {
    conditions: [] as string[],
    meds: "",
  },
  contact: {
    name: "",
    email: "",
    phone: "",
  },
})

const progressPercent = computed(() => (currentStep.value / totalSteps) * 100)

const stepLabel = computed(() => {
  if (currentStep.value === 1) return "Reason for visit"
  if (currentStep.value === 2) return "Your health history"
  return "Contact and appointment details"
})

const onNext = () => {
  if (currentStep.value < totalSteps) currentStep.value++
}

const onBack = () => {
  if (currentStep.value > 1) currentStep.value--
}

const handleSubmit = () => {
  // plug in actual submit logic here
  console.log("Submitting intake:", JSON.parse(JSON.stringify(formData)))
}
</script>

<template>
  <section
    class="flex-1 rounded-3xl bg-white p-6 shadow-xl shadow-cyan-100/60 ring-1 ring-slate-100 dark:bg-slate-900 dark:ring-slate-800 dark:shadow-black/40"
    aria-labelledby="intake-heading"
  >
    <header class="mb-6 space-y-2">
      <p class="text-xs font-medium uppercase tracking-[0.18em] text-brand-teal dark:text-brand-accent">
        HealthMD intake
      </p>

      <div class="flex items-center justify-between gap-4">
        <div>
          <h1
            id="intake-heading"
            class="text-2xl font-semibold tracking-tight text-brand-primary dark:text-white sm:text-3xl"
          >
            What brings you in today?
          </h1>
          <p class="mt-1 text-sm text-slate-600 dark:text-slate-300">
            This helps us match you with the right clinician and plan.
          </p>
        </div>

        <div class="hidden text-right text-sm text-slate-500 dark:text-slate-300 sm:block">
          <p class="font-semibold text-brand-primary dark:text-brand-chip">
            Step {{ currentStep }} of {{ totalSteps }}
          </p>
          <p>
            {{ stepLabel }}
          </p>
        </div>
      </div>

      <div
        class="mt-4 h-1.5 w-full overflow-hidden rounded-full bg-slate-100 dark:bg-slate-800"
        role="progressbar"
        :aria-valuenow="progressPercent"
        aria-valuemin="0"
        aria-valuemax="100"
      >
        <div
          class="h-full bg-gradient-to-r from-brand-teal via-brand-accent to-brand-deep transition-[width]"
          :style="{ width: progressPercent + '%' }"
        />
      </div>
    </header>

    <form
      class="space-y-6"
      @submit.prevent="currentStep === totalSteps ? handleSubmit() : null"
    >
      <IntakeStepReason
        v-if="currentStep === 1"
        v-model:reason="formData.reason"
        @next="onNext"
      />

      <IntakeStepHistory
        v-else-if="currentStep === 2"
        v-model:history="formData.history"
        @next="onNext"
        @back="onBack"
      />

      <IntakeStepDetails
        v-else
        v-model:contact="formData.contact"
        @back="onBack"
      />
    </form>
  </section>
</template>
