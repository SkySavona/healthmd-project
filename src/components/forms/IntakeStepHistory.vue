<script setup lang="ts">
import Button from "@/components/ui/Button.vue"

type History = {
  conditions: string[]
  meds: string
}

const props = defineProps<{
  history: History
}>()

const emit = defineEmits<{
  (e: "update:history", value: History): void
  (e: "next"): void
  (e: "back"): void
}>()

const conditionsList = [
  { id: "diabetes", label: "Diabetes or prediabetes" },
  { id: "hypertension", label: "High blood pressure" },
  { id: "heart", label: "Heart disease" },
  { id: "sleep-apnea", label: "Sleep apnea" },
  { id: "thyroid", label: "Thyroid or hormone issues" },
  { id: "none", label: "None of these" },
]

const toggleCondition = (id: string, checked: boolean) => {
  const current = new Set(props.history.conditions)

  if (id === "none") {
    // if selecting "none", clear everything else
    if (checked) {
      emit("update:history", {
        ...props.history,
        conditions: ["none"],
      })
    } else {
      emit("update:history", {
        ...props.history,
        conditions: [],
      })
    }
    return
  }

  // remove "none" if selecting any real condition
  current.delete("none")

  if (checked) {
    current.add(id)
  } else {
    current.delete(id)
  }

  emit("update:history", {
    ...props.history,
    conditions: Array.from(current),
  })
}

const updateMeds = (value: string) => {
  emit("update:history", {
    ...props.history,
    meds: value,
  })
}
</script>

<template>
  <div class="space-y-8">
    <fieldset
      class="space-y-4"
      aria-describedby="conditions-helper"
    >
      <legend class="text-sm font-semibold text-slate-900 dark:text-slate-100">
        Do you have any of the following conditions?
      </legend>

      <p
        id="conditions-helper"
        class="text-sm text-slate-500 dark:text-slate-300"
      >
        This helps your clinician understand your risk factors. Select all that apply.
      </p>

      <div class="grid gap-3 md:grid-cols-2">
        <label
          v-for="condition in conditionsList"
          :key="condition.id"
          class="flex cursor-pointer items-start gap-3 rounded-2xl border border-slate-200 bg-slate-50/60 p-3 text-sm shadow-sm transition hover:bg-white hover:shadow-md hover:shadow-cyan-50 hover:ring-1 hover:ring-brand-teal dark:border-slate-700 dark:bg-slate-900/70 dark:hover:bg-slate-900 dark:hover:shadow-lg dark:hover:shadow-brand-primary/30 dark:hover:ring-brand-accent"
        >
          <input
            type="checkbox"
            class="mt-1 h-4 w-4 rounded border-slate-300 text-brand-teal focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-brand-teal focus-visible:ring-offset-2 focus-visible:ring-offset-white dark:border-slate-500 dark:bg-slate-900 dark:text-brand-accent dark:focus-visible:ring-brand-accent dark:focus-visible:ring-offset-slate-950"
            :checked="history.conditions.includes(condition.id)"
            :value="condition.id"
            @change="toggleCondition(condition.id, ($event.target as HTMLInputElement).checked)"
          />

          <span class="flex flex-col">
            <span class="font-medium text-slate-900 dark:text-slate-100">
              {{ condition.label }}
            </span>
            <span
              v-if="condition.id === 'none'"
              class="text-xs text-slate-500 dark:text-slate-400"
            >
              If checked, we will treat this as no major chronic conditions.
            </span>
          </span>
        </label>
      </div>
    </fieldset>

    <fieldset class="space-y-3">
      <legend class="text-sm font-semibold text-slate-900 dark:text-slate-100">
        Current medications
      </legend>

      <p class="text-sm text-slate-500 dark:text-slate-300">
        List any prescription or over the counter medications you are taking, especially for weight, blood pressure, or mood.
      </p>

      <textarea
        name="medications"
        rows="4"
        class="block w-full rounded-2xl border border-slate-200 bg-slate-50/60 px-3 py-2 text-sm text-slate-900 placeholder:text-slate-400 shadow-sm focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-brand-teal focus-visible:ring-offset-2 focus-visible:ring-offset-white dark:border-slate-700 dark:bg-slate-900/60 dark:text-slate-50 dark:placeholder:text-slate-500 dark:focus-visible:ring-brand-accent dark:focus-visible:ring-offset-slate-950"
        placeholder="For example: metformin, Ozempic, blood pressure medicine, antidepressants..."
        :value="history.meds"
        @input="updateMeds(($event.target as HTMLTextAreaElement).value)"
      />
    </fieldset>

    <div class="mt-4 flex flex-wrap items-center justify-between gap-4">
      <Button
        type="button"
        variant="secondary"
        @click="emit('back')"
      >
        Back
      </Button>

      <Button
        type="button"
        variant="primary"
        @click="emit('next')"
      >
        Next: Contact and visit details →
      </Button>
    </div>
  </div>
</template>
