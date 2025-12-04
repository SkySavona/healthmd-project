<script setup lang="ts">
import { reactive, ref, computed } from "vue"
import IntakeStepReason from "@/components/forms/IntakeStepReason.vue"
import IntakeStepHistory from "@/components/forms/IntakeStepHistory.vue"
import IntakeStepDetails from "@/components/forms/IntakeStepDetails.vue"
import IntakeStepReview from "@/components/forms/IntakeStepReview.vue"
import IntakeStepConfirmation from "./IntakeStepConfirmation.vue"

const currentStep = ref(1)
const totalSteps = 5

const formData = reactive({
  reason: "",
  history: {
    conditions: [] as string[],
    meds: "",
  },
  contact: {
    name: "",
    email: "",
    phone: "",
    note: "",
  },
})

const progressPercent = computed(() => (currentStep.value / totalSteps) * 100)

const stepLabel = computed(() => {
  if (currentStep.value === 1) return "Reason for visit"
  if (currentStep.value === 2) return "Your health history"
  if (currentStep.value === 3) return "Contact details"
  if (currentStep.value === 4) return "Review and submit"
  return "Confirmation"
})

const stepTitle = computed(() => {
  if (currentStep.value === 1) return "What brings you in today?"
  if (currentStep.value === 2) return "Tell us about your health history"
  if (currentStep.value === 3) return "How can we reach you?"
  if (currentStep.value === 4) return "Review your details before you submit"
  return "You are all set"
})

const stepDescription = computed(() => {
  if (currentStep.value === 1) {
    return "This helps us match you with the right clinician and care plan."
  }
  if (currentStep.value === 2) {
    return "A brief medical snapshot helps your clinician tailor safe, effective care."
  }
  if (currentStep.value === 3) {
    return "We will use this contact info to share appointment details and secure visit links."
  }
  if (currentStep.value === 4) {
    return "Double check everything looks right before you send your intake to the team."
  }
  return "We have received your intake. Our clinical team will follow up with next steps."
})

const onNext = () => {
  if (currentStep.value < totalSteps) currentStep.value++
}

const onBack = () => {
  if (currentStep.value > 1) currentStep.value--
}
const progressLabel = computed(
  () => `Intake progress: step ${currentStep.value} of ${totalSteps}`
)

const progressValueText = computed(
  () => `Step ${currentStep.value} of ${totalSteps}`
)

const resetForm = () => {
  currentStep.value = 1
  formData.reason = ""
  formData.history.conditions = []
  formData.history.meds = ""
  formData.contact.name = ""
  formData.contact.email = ""
  formData.contact.phone = ""
  formData.contact.note = ""
}

const handleSubmit = () => {
  console.log("Submitting intake:", JSON.parse(JSON.stringify(formData)))
  currentStep.value = 5
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
            {{ stepTitle }}
          </h1>
          <p class="mt-1 text-sm text-slate-600 dark:text-slate-300">
            {{ stepDescription }}
          </p>
        </div>

        <div class="hidden text-right text-sm text-slate-600 dark:text-slate-300 sm:block">
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
  :aria-label="progressLabel"
  :aria-valuetext="progressValueText"
>
  <div
    class="h-full bg-gradient-to-r from-brand-teal via-brand-accent to-brand-deep motion-safe:transition-[width]"
    :style="{ width: progressPercent + '%' }"
  />
</div>

    </header>

    <form
      class="space-y-6"
      @submit.prevent="currentStep === 4 ? handleSubmit() : null"
    >
      <Transition name="intake-step" mode="out-in">
        <div :key="currentStep">
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
            v-else-if="currentStep === 3"
            v-model:contact="formData.contact"
            @next="onNext"
            @back="onBack"
          />

          <IntakeStepReview
            v-else-if="currentStep === 4"
            :reason="formData.reason"
            :history="formData.history"
            :contact="formData.contact"
            @back="onBack"
          />

          <IntakeStepConfirmation
            v-else
            :contact="formData.contact"
            @startOver="resetForm"
          />
        </div>
      </Transition>
    </form>
  </section>
</template>

<style scoped>
.intake-step-enter-active,
.intake-step-leave-active {
  transition: opacity 200ms ease-out, transform 200ms ease-out;
}

.intake-step-enter-from,
.intake-step-leave-to {
  opacity: 0;
  transform: translateY(8px);
}

.intake-step-enter-to,
.intake-step-leave-from {
  opacity: 1;
  transform: translateY(0);
}

@media (prefers-reduced-motion: reduce) {
  .intake-step-enter-active,
  .intake-step-leave-active {
    transition: none;
  }

  .intake-step-enter-from,
  .intake-step-leave-to {
    opacity: 1;
    transform: none;
  }
}
</style>
