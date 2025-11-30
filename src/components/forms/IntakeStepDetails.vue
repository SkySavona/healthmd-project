<script setup lang="ts">
import Button from "@/components/ui/Button.vue"

type Contact = {
  name: string
  email: string
  phone: string
}

const props = defineProps<{
  contact: Contact
}>()

const emit = defineEmits<{
  (e: "update:contact", value: Contact): void
  (e: "back"): void
}>()

const updateField = (field: keyof Contact, value: string) => {
  emit("update:contact", {
    ...props.contact,
    [field]: value,
  })
}
</script>

<template>
  <div class="space-y-8">
    <fieldset class="space-y-4">
      <legend class="text-sm font-semibold text-slate-900 dark:text-slate-100">
        How can we reach you?
      </legend>

      <p class="text-sm text-slate-500 dark:text-slate-300">
        We use this info to send appointment details and secure visit links.
      </p>

      <div class="grid gap-4 md:grid-cols-2">
        <div class="space-y-1.5">
          <label
            for="contact-name"
            class="block text-sm font-medium text-slate-900 dark:text-slate-100"
          >
            Full name
          </label>
          <input
            id="contact-name"
            name="name"
            type="text"
            autocomplete="name"
            class="block w-full rounded-2xl border border-slate-200 bg-slate-50/60 px-3 py-2 text-sm text-slate-900 placeholder:text-slate-400 shadow-sm focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-brand-teal focus-visible:ring-offset-2 focus-visible:ring-offset-white dark:border-slate-700 dark:bg-slate-900/60 dark:text-slate-50 dark:placeholder:text-slate-500 dark:focus-visible:ring-brand-accent dark:focus-visible:ring-offset-slate-950"
            placeholder="Jane Doe"
            :value="contact.name"
            @input="updateField('name', ($event.target as HTMLInputElement).value)"
          />
        </div>

        <div class="space-y-1.5">
          <label
            for="contact-phone"
            class="block text-sm font-medium text-slate-900 dark:text-slate-100"
          >
            Mobile number
          </label>
          <input
            id="contact-phone"
            name="phone"
            type="tel"
            autocomplete="tel"
            class="block w-full rounded-2xl border border-slate-200 bg-slate-50/60 px-3 py-2 text-sm text-slate-900 placeholder:text-slate-400 shadow-sm focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-brand-teal focus-visible:ring-offset-2 focus-visible:ring-offset-white dark:border-slate-700 dark:bg-slate-900/60 dark:text-slate-50 dark:placeholder:text-slate-500 dark:focus-visible:ring-brand-accent dark:focus-visible:ring-offset-slate-950"
            placeholder="(555) 555-5555"
            :value="contact.phone"
            @input="updateField('phone', ($event.target as HTMLInputElement).value)"
          />
        </div>
      </div>

      <div class="space-y-1.5">
        <label
          for="contact-email"
          class="block text-sm font-medium text-slate-900 dark:text-slate-100"
        >
          Email address
        </label>
        <input
          id="contact-email"
          name="email"
          type="email"
          autocomplete="email"
          class="block w-full rounded-2xl border border-slate-200 bg-slate-50/60 px-3 py-2 text-sm text-slate-900 placeholder:text-slate-400 shadow-sm focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-brand-teal focus-visible:ring-offset-2 focus-visible:ring-offset-white dark:border-slate-700 dark:bg-slate-900/60 dark:text-slate-50 dark:placeholder:text-slate-500 dark:focus-visible:ring-brand-accent dark:focus-visible:ring-offset-slate-950"
          placeholder="you@example.com"
          :value="contact.email"
          @input="updateField('email', ($event.target as HTMLInputElement).value)"
        />
      </div>
    </fieldset>

    <fieldset class="space-y-3">
      <legend class="text-sm font-semibold text-slate-900 dark:text-slate-100">
        Anything else you want us to know?
      </legend>

      <p class="text-sm text-slate-500 dark:text-slate-300">
        You can share goals, timing preferences, or any concerns you would like your clinician to see first.
      </p>

      <textarea
        name="note"
        rows="4"
        class="block w-full rounded-2xl border border-slate-200 bg-slate-50/60 px-3 py-2 text-sm text-slate-900 placeholder:text-slate-400 shadow-sm focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-brand-teal focus-visible:ring-offset-2 focus-visible:ring-offset-white dark:border-slate-700 dark:bg-slate-900/60 dark:text-slate-50 dark:placeholder:text-slate-500 dark:focus-visible:ring-brand-accent dark:focus-visible:ring-offset-slate-950"
        placeholder="Optional: Tell us about your goals, timing, or anything important to you."
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
        type="submit"
        variant="primary"
      >
        Submit intake and continue →
      </Button>
    </div>
  </div>
</template>
