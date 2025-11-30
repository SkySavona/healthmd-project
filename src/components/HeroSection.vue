<script setup>
import { ref } from "vue"
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome"
import {
  faWeightScale,
  faClinicMedical,
  faPrescriptionBottleMedical,
  faStethoscope,
} from "@fortawesome/free-solid-svg-icons"
import Button from "./ui/Button.vue"

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

const selectedReason = ref("weight-long-term")
</script>

<template>
  <main
    class="mx-auto flex min-h-screen max-w-6xl flex-col gap-10 px-4 pb-12 pt-8 lg:flex-row lg:items-stretch lg:px-8"
  >
    <section
      class="flex-1 rounded-3xl bg-white p-6 shadow-xl shadow-cyan-100/60 ring-1 ring-slate-100"
      aria-labelledby="intake-heading"
    >
      <header class="mb-6 space-y-2">
        <p class="text-xs font-medium uppercase tracking-[0.18em] text-brand-teal">
          HealthMD intake
        </p>

        <div class="flex items-center justify-between gap-4">
          <div>
            <h1
              id="intake-heading"
              class="text-2xl font-semibold tracking-tight text-brand-primary sm:text-3xl"
            >
              What brings you in today?
            </h1>
            <p class="mt-1 text-sm text-slate-600">
              This helps us match you with the right clinician and plan.
            </p>
          </div>

          <div class="hidden text-right text-sm text-slate-500 sm:block">
            <p class="font-semibold text-brand-primary">Step 1 of 3</p>
            <p>Reason for visit</p>
          </div>
        </div>

        <div
          class="mt-4 h-1.5 w-full overflow-hidden rounded-full bg-slate-100"
          role="progressbar"
          aria-valuenow="33"
          aria-valuemin="0"
          aria-valuemax="100"
        >
          <div
            class="h-full w-1/3 bg-gradient-to-r from-brand-teal via-brand-accent to-brand-deep"
          />
        </div>
      </header>

      <form class="space-y-6">
        <input type="hidden" name="reason" :value="selectedReason" />

        <fieldset
          class="space-y-4"
          aria-describedby="reason-helper"
        >
          <legend class="sr-only">
            Select a reason for your visit
          </legend>

          <p id="reason-helper" class="text-sm text-slate-500">
            Choose the one that feels closest. You can add more detail later.
          </p>

          <div
            role="radiogroup"
            aria-label="Reason for visit"
            class="grid gap-4 md:grid-cols-2"
          >
            <button
              v-for="reason in reasons"
              :key="reason.id"
              type="button"
              role="radio"
              :aria-checked="selectedReason === reason.id"
              @click="selectedReason = reason.id"
              @keydown.enter.prevent="selectedReason = reason.id"
              @keydown.space.prevent="selectedReason = reason.id"
              :class="[
                'group flex w-full flex-col gap-2 rounded-2xl border bg-slate-50/60 p-4 text-left text-sm shadow-sm transition ',
                'hover:bg-white hover:shadow-md hover:shadow-cyan-50 hover:ring-1 hover:ring-brand-teal',
                'focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-brand-teal focus-visible:ring-offset-2 focus-visible:ring-offset-white cursor-pointer',
                selectedReason === reason.id
                  ? 'border-brand-teal border-2 bg-white shadow-md shadow-cyan-100'
                  : 'border-slate-200'
              ]"
            >
              <div class="flex items-center justify-between gap-3">
                <div
                  class="flex items-center gap-3"
                >
                  <span
                    class="flex h-5 w-5 items-center justify-center rounded-full border-2"
                    :class="selectedReason === reason.id
                      ? 'border-brand-deep'
                      : 'border-slate-300'"
                    aria-hidden="true"
                  >
                    <span
                      v-if="selectedReason === reason.id"
                      class="h-2.5 w-2.5 rounded-full bg-brand-deep"
                    />
                  </span>

                  <span
                    class="rounded-full bg-slate-100 px-3 py-1 text-[0.72rem] font-medium uppercase tracking-wide text-slate-500 group-hover:bg-brand-chip group-focus:bg-brand-chip"
                  >
                    {{ reason.chip }}
                  </span>
                </div>

                <FontAwesomeIcon
                  :icon="reason.icon"
                  class="text-lg text-brand-teal"
                  aria-hidden="true"
                />
              </div>

              <p class="mt-1 text-sm font-semibold text-brand-primary group-hover:text-brand-deep group-focus:text-brand-deep">
                {{ reason.label }}
              </p>

              <p class="text-sm text-slate-600">
                {{ reason.desc }}
              </p>
            </button>
          </div>
        </fieldset>

        <div class="mt-4 flex flex-wrap items-center justify-between gap-4">
          <p class="text-sm text-slate-500">
            You can change this later in your visit summary.
          </p>

        <Button type="submit" variant="primary">
          Next: Your health history →  </Button>
        </div>
      </form>
    </section>
  </main>
</template>
