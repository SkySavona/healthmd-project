<script setup lang="ts">
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome"
import {
  faWeightScale,
  faClinicMedical,
  faPrescriptionBottleMedical,
  faStethoscope,
} from "@fortawesome/free-solid-svg-icons"
import Button from "@/components/ui/Button.vue"

const props = defineProps<{
  reason: string
}>()

const emit = defineEmits<{
  (e: "update:reason", value: string): void
  (e: "next"): void
}>()

const reasons = [
  {
    id: "weight-long-term",
    label: "I want help with long term weight loss",
    desc: "You want a sustainable plan with clinical support and realistic goals.",
    icon: faWeightScale,
    chip: "Weight Managment",
  },
  {
    id: "med-refill",
    label: "I need a medication refill or review",
    desc: "You need renewals or help with current meds.",
    icon: faPrescriptionBottleMedical,
    chip: "Medications",
  },
  {
    id: "urgent-care",
    label: "I have an urgent or new health concern",
    desc: "Quick care for infections, cold and flu, minor issues.",
    icon: faClinicMedical,
    chip: "Urgent care",
  },
  {
    id: "primary-care",
    label: "I want primary care support",
    desc: "Ongoing management and preventative care.",
    icon: faStethoscope,
    chip: "Primary care",
  },
]

const selectReason = (id: string) => {
  emit("update:reason", id)
}
</script>

<template>
  <div class="space-y-6">
    <!-- keeps value in the eventual form post -->
    <input type="hidden" name="reason" :value="reason" />

    <fieldset
      class="space-y-4"
      aria-describedby="reason-helper"
    >
      <legend class="sr-only">
        Select a reason for your visit
      </legend>

      <div class="flex items-baseline gap-2">
        <p
          id="reason-label"
          class="text-sm font-medium text-slate-700 dark:text-slate-200"
        >
          Reason for your visit
          <span class="text-rose-700 dark:text-rose-400">*Required</span>
          <span class="sr-only">(required)</span>
        </p>
      </div>

      <p
        id="reason-helper"
        class="text-sm text-slate-600 dark:text-slate-300"
      >
        Choose the one that feels closest. You can add more detail later.
      </p>

      <div
        role="radiogroup"
        aria-labelledby="reason-label"
        aria-describedby="reason-helper"
        aria-required="true"
        class="grid gap-4 md:grid-cols-2"
      >
        <button
          v-for="item in reasons"
          :key="item.id"
          type="button"
          role="radio"
          :aria-checked="reason === item.id"
          @click="selectReason(item.id)"
          @keydown.enter.prevent="selectReason(item.id)"
          @keydown.space.prevent="selectReason(item.id)"
          :class="[
            'group flex w-full flex-col gap-2 rounded-2xl border bg-slate-50/60 p-4 text-left text-sm shadow-sm transition',
            'hover:bg-white hover:shadow-md hover:shadow-cyan-50 hover:ring-1 hover:ring-brand-teal',
            'dark:bg-slate-900/70 dark:border-slate-700 dark:hover:bg-slate-900 dark:hover:shadow-lg dark:hover:shadow-brand-primary/30 dark:hover:ring-brand-accent',
            'focus:outline-none focus:ring-1 focus:ring-brand-teal focus:ring-offset-white',
            'dark:focus:bg-slate-900 dark:focus:shadow-lg dark:focus:shadow-brand-primary/30 dark:focus:ring-1 dark:focus:ring-brand-accent dark:focus:ring-offset-slate-950',
            reason === item.id
              ? 'border-brand-teal bg-white shadow-md shadow-cyan-100 dark:bg-brand-primary dark:border-brand-accent dark:shadow-brand-primary/50'
              : 'border-slate-200 dark:border-slate-700',
          ]"
        >
          <div class="flex items-center justify-between gap-3">
            <div class="flex items-center gap-3">
              <!-- custom radio dot -->
              <span
                class="flex h-5 w-5 items-center justify-center rounded-full border-2"
                :class="reason === item.id
                  ? 'border-brand-deep dark:border-brand-chip'
                  : 'border-slate-300 dark:border-slate-500'"
                aria-hidden="true"
              >
                <span
                  v-if="reason === item.id"
                  class="h-2.5 w-2.5 rounded-full bg-brand-deep dark:bg-brand-chip"
                />
              </span>

              <!-- chip -->
              <span
                class="rounded-full bg-slate-100 px-3 py-1 text-[0.72rem] font-medium uppercase tracking-wide text-slate-700 group-hover:bg-brand-chip group-focus:bg-brand-chip dark:bg-slate-800 dark:text-slate-300 dark:group-hover:bg-slate-700 dark:group-focus:bg-slate-700"
              >
                {{ item.chip }}
              </span>
            </div>

            <FontAwesomeIcon
              :icon="item.icon"
              class="text-lg text-brand-teal dark:text-brand-accent"
              aria-hidden="true"
            />
          </div>

          <p
            class="mt-1 text-sm font-semibold text-brand-primary group-hover:text-brand-deep group-focus:text-brand-deep dark:text-brand-chip dark:group-hover:text-white dark:group-focus:text-white"
          >
            {{ item.label }}
          </p>

          <p class="text-sm text-slate-600 dark:text-slate-300">
            {{ item.desc }}
          </p>
        </button>
      </div>
    </fieldset>

    <div class="mt-4 flex flex-wrap items-center justify-between gap-4">
      <p class="text-sm text-slate-600 dark:text-slate-300">
        You can change this later in your visit summary.
      </p>

      <Button
        type="button"
        variant="primary"
        :disabled="!reason"
        @click="emit('next')"
      >
        Next: Your health history →
      </Button>
    </div>
  </div>
</template>
