<script setup lang="ts">
import Button from "@/components/ui/Button.vue"

type History = {
  conditions: string[]
  meds: string
}

type Contact = {
  name: string
  email: string
  phone: string
  note: string
}

const props = defineProps<{
  reason: string
  history: History
  contact: Contact
}>()

const emit = defineEmits<{
  (e: "back"): void
}>()

const reasonLabelMap: Record<string, string> = {
  "weight-long-term": "Long term weight loss support",
  "med-refill": "Medication refill or review",
  "urgent-care": "Urgent or new health concern",
  "primary-care": "Primary care and ongoing support",
}

const reasonChipMap: Record<string, string> = {
  "weight-long-term": "Weight management",
  "med-refill": "Medications",
  "urgent-care": "Urgent care",
  "primary-care": "Primary care",
}

const displayReasonLabel = (id: string) => {
  return reasonLabelMap[id] ?? "Reason for visit"
}

const displayReasonChip = (id: string) => {
  return reasonChipMap[id] ?? "Visit type"
}

const hasConditions = (history: History) => {
  return history.conditions.length > 0 && !history.conditions.includes("none")
}
</script>

<template>
  <div class="space-y-8">
    <header class="space-y-2">
      <h2 class="text-lg font-semibold text-slate-900 dark:text-slate-50">
        Review your details
      </h2>
      <p class="text-sm text-slate-600 dark:text-slate-300">
        Take a moment to confirm everything looks right before you send your intake to the clinical team.
      </p>
    </header>

    <!-- Reason summary -->
    <section
      class="rounded-2xl border border-slate-200 bg-slate-50/70 p-4 shadow-sm dark:border-slate-700 dark:bg-slate-900/70"
      aria-labelledby="summary-reason-heading"
    >
      <div class="flex items-start justify-between gap-4">
        <div>
          <h3
            id="summary-reason-heading"
            class="text-sm font-semibold text-slate-900 dark:text-slate-50"
          >
            Reason for visit
          </h3>
          <p class="mt-1 text-sm text-slate-600 dark:text-slate-300">
            {{ displayReasonLabel(reason) }}
          </p>
        </div>

        <span
          class="mt-1 inline-flex items-center rounded-full bg-brand-chip px-3 py-1 text-[0.72rem] font-medium uppercase tracking-wide text-brand-primary dark:bg-slate-800 dark:text-slate-100"
        >
          {{ displayReasonChip(reason) }}
        </span>
      </div>
    </section>

    <!-- History summary -->
    <section
      class="rounded-2xl border border-slate-200 bg-slate-50/70 p-4 shadow-sm dark:border-slate-700 dark:bg-slate-900/70"
      aria-labelledby="summary-history-heading"
    >
      <h3
        id="summary-history-heading"
        class="text-sm font-semibold text-slate-900 dark:text-slate-50"
      >
        Health history
      </h3>

      <div class="mt-3 space-y-3 text-sm">
        <div>
          <p class="font-medium text-slate-800 dark:text-slate-100">
            Conditions
          </p>

          <div class="mt-1">
            <template v-if="hasConditions(history)">
              <ul class="flex flex-wrap gap-2">
                <li
                  v-for="condition in history.conditions"
                  :key="condition"
                  class="mt-1 inline-flex items-center rounded-full bg-slate-100 px-3 py-1 text-[0.72rem] font-medium uppercase tracking-wide text-slate-700 dark:bg-slate-800 dark:text-slate-100"
                >
                  {{ condition }}
                </li>
              </ul>
            </template>

            <p
              v-else-if="history.conditions.includes('none')"
              class="text-slate-600 dark:text-slate-300"
            >
              You indicated that you do not have major chronic conditions from our list.
            </p>

            <p
              v-else
              class="text-slate-600 dark:text-slate-300"
            >
              No conditions selected.
            </p>
          </div>
        </div>

        <div>
          <p class="font-medium text-slate-800 dark:text-slate-100">
            Medications
          </p>

          <p
            v-if="history.meds.trim().length"
            class="mt-1 whitespace-pre-wrap break-words text-slate-700 dark:text-slate-200"
          >
            {{ history.meds }}
          </p>
          <p
            v-else
            class="mt-1 text-slate-600 dark:text-slate-300"
          >
            No medications listed.
          </p>
        </div>
      </div>
    </section>

    <!-- Contact summary -->
    <section
      class="rounded-2xl border border-slate-200 bg-slate-50/70 p-4 shadow-sm dark:border-slate-700 dark:bg-slate-900/70"
      aria-labelledby="summary-contact-heading"
    >
      <h3
        id="summary-contact-heading"
        class="text-sm font-semibold text-slate-900 dark:text-slate-50"
      >
        Contact details
      </h3>

      <dl class="mt-3 grid gap-3 text-sm sm:grid-cols-2">
        <div>
          <dt class="text-slate-500 dark:text-slate-400">
            Name
          </dt>
          <dd class="text-slate-900 dark:text-slate-100">
            {{ contact.name || "Not provided" }}
          </dd>
        </div>

        <div>
          <dt class="text-slate-500 dark:text-slate-400">
            Mobile number
          </dt>
          <dd class="text-slate-900 dark:text-slate-100">
            {{ contact.phone || "Not provided" }}
          </dd>
        </div>

        <div class="sm:col-span-2">
          <dt class="text-slate-500 dark:text-slate-400">
            Email
          </dt>
          <dd class="text-slate-900 dark:text-slate-100">
            {{ contact.email || "Not provided" }}
          </dd>
        </div>

        <div class="sm:col-span-2">
          <dt class="text-slate-500 dark:text-slate-400">
            Message for your clinician
          </dt>
          <dd
            class="text-slate-900 dark:text-slate-100 whitespace-pre-wrap break-words"
          >
            {{ contact.note || "No additional notes provided." }}
          </dd>
        </div>
      </dl>
    </section>

    <p class="text-sm text-slate-600 dark:text-slate-400">
      By submitting, you agree to receive visit reminders and clinical messages related to your care.
    </p>

    <div class="mt-4 flex flex-col sm:flex-row sm:justify-between items-stretch gap-4">
      <Button
        type="button"
        variant="secondary"
        @click="emit('back')"
      >
        ← Back
      </Button>

      <Button
        type="submit"
        variant="primary"
      >
        Submit intake →
      </Button>
    </div>
  </div>
</template>
