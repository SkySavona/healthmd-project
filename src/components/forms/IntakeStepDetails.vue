<script setup lang="ts">
import { ref, computed, nextTick } from "vue"
import Button from "@/components/ui/Button.vue"

type Contact = {
  name: string
  email: string
  phone: string
  note: string
}

const props = defineProps<{
  contact: Contact
}>()

const emit = defineEmits<{
  (e: "update:contact", value: Contact): void
  (e: "back"): void
  (e: "next"): void
}>()

const nameInput = ref<HTMLInputElement | null>(null)
const phoneInput = ref<HTMLInputElement | null>(null)
const emailInput = ref<HTMLInputElement | null>(null)

const errors = ref<{
  name?: string
  email?: string
  phone?: string
}>({})

const hasErrors = computed(() => Object.keys(errors.value).length > 0)

const updateField = (field: keyof Contact, value: string) => {
  if (errors.value[field]) {
    const newErrors = { ...errors.value }
    delete newErrors[field]
    errors.value = newErrors
  }

  emit("update:contact", {
    ...props.contact,
    [field]: value,
  })
}

const basicEmail = /^[^\s@]+@[^\s@]+\.[^\s@]+$/

const isFormValid = computed(() => {
  const name = props.contact.name.trim()
  const email = props.contact.email.trim()
  const phoneRaw = props.contact.phone.trim()
  const phoneDigits = phoneRaw.replace(/\D/g, "")

  if (!name || !email || !phoneRaw) return false
  if (!basicEmail.test(email)) return false
  if (phoneDigits.length < 10) return false

  return true
})

const validate = async () => {
  const newErrors: typeof errors.value = {}

  const name = props.contact.name.trim()
  const email = props.contact.email.trim()
  const phoneRaw = props.contact.phone.trim()
  const phoneDigits = phoneRaw.replace(/\D/g, "")

  if (!name) {
    newErrors.name = "Enter your full name, like Jane Doe."
  }

  if (!email) {
    newErrors.email = "Enter your email address, like you@example.com."
  } else if (!basicEmail.test(email)) {
    newErrors.email = "Enter a valid email address, like name@example.com."
  }

  if (!phoneRaw) {
    newErrors.phone = "Enter your mobile number, like 555-555-5555."
  } else if (phoneDigits.length < 10) {
    newErrors.phone =
      "Your mobile number must be at least 10 digits, like 555-555-5555."
  }

  errors.value = newErrors

  if (Object.keys(newErrors).length === 0) {
    return true
  }

  await nextTick()
  if (newErrors.name && nameInput.value) {
    nameInput.value.focus()
  } else if (newErrors.phone && phoneInput.value) {
    phoneInput.value.focus()
  } else if (newErrors.email && emailInput.value) {
    emailInput.value.focus()
  }

  return false
}

const handleNext = async () => {
  const ok = await validate()
  if (ok) {
    emit("next")
  }
}
</script>

<template>
  <div class="space-y-8">
    <fieldset
      class="space-y-4"
      aria-describedby="contact-helper"
    >
      <legend class="text-sm font-semibold text-slate-900 dark:text-slate-100">
        How can we reach you?
      </legend>

      <p
        id="contact-helper"
        class="text-sm text-slate-500 dark:text-slate-300"
      >
        We use this info to send appointment details and secure visit links.
      </p>

      <p
        v-if="hasErrors"
        id="contact-errors"
        class="text-sm font-medium text-rose-700 dark:text-rose-400"
        role="alert"
      >
        There is a problem with some of the contact details. Check the fields marked in red and follow the examples in each label.
      </p>

      <div class="grid gap-4 md:grid-cols-2">
        <!-- Full name -->
        <div class="space-y-1.5">
          <label
            for="contact-name"
            class="flex items-baseline justify-between gap-2 text-sm font-medium text-slate-900 dark:text-slate-100"
          >
            <span class="flex items-baseline gap-1">
              <span>Full name</span>
              <span class="text-rose-700 dark:text-rose-400">
                *Required
              </span>
            </span>

            <span class="text-sm font-normal text-slate-500 dark:text-slate-300">
              Example: Jane Doe
            </span>
          </label>

          <input
            id="contact-name"
            ref="nameInput"
            name="name"
            type="text"
            required
            autocomplete="name"
            :aria-invalid="!!errors.name"
            :aria-describedby="errors.name ? 'contact-name-error' : undefined"
            class="block w-full rounded-2xl border bg-slate-50/60 px-3 py-2 text-sm text-slate-900 placeholder:text-slate-400 shadow-sm focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-brand-teal focus-visible:ring-offset-2 focus-visible:ring-offset-white dark:bg-slate-900/60 dark:text-slate-50 dark:placeholder:text-slate-500 dark:focus-visible:ring-brand-accent dark:focus-visible:ring-offset-slate-950"
            :class="errors.name
              ? 'border-rose-600 dark:border-rose-500'
              : 'border-slate-200 dark:border-slate-700'"
            placeholder="Jane Doe"
            :value="contact.name"
            @input="updateField('name', ($event.target as HTMLInputElement).value)"
          />

          <p
            v-if="errors.name"
            id="contact-name-error"
            class="text-xs text-rose-700 dark:text-rose-400"
            role="alert"
          >
            {{ errors.name }}
          </p>
        </div>

        <!-- Mobile number -->
        <div class="space-y-1.5">
          <label
            for="contact-phone"
            class="flex items-baseline justify-between gap-2 text-sm font-medium text-slate-900 dark:text-slate-100"
          >
            <span class="flex items-baseline gap-1">
              <span>Mobile number</span>
              <span class="text-rose-700 dark:text-rose-400">
                *Required
              </span>
            </span>

            <span class="text-sm font-normal text-slate-500 dark:text-slate-300">
              Example: (555) 555-5555
            </span>
          </label>

          <input
            id="contact-phone"
            ref="phoneInput"
            name="phone"
            type="tel"
            required
            autocomplete="tel"
            inputmode="tel"
            :aria-invalid="!!errors.phone"
            :aria-describedby="errors.phone ? 'contact-phone-error' : undefined"
            class="block w-full rounded-2xl border bg-slate-50/60 px-3 py-2 text-sm text-slate-900 placeholder:text-slate-400 shadow-sm focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-brand-teal focus-visible:ring-offset-2 focus-visible:ring-offset-white dark:bg-slate-900/60 dark:text-slate-50 dark:placeholder:text-slate-500 dark:focus-visible:ring-brand-accent dark:focus-visible:ring-offset-slate-950"
            :class="errors.phone
              ? 'border-rose-600 dark:border-rose-500'
              : 'border-slate-200 dark:border-slate-700'"
            placeholder="(555) 555-5555"
            :value="contact.phone"
            @input="updateField('phone', ($event.target as HTMLInputElement).value)"
          />

          <p
            v-if="errors.phone"
            id="contact-phone-error"
            class="text-xs text-rose-700 dark:text-rose-400"
            role="alert"
          >
            {{ errors.phone }}
          </p>
        </div>
      </div>

      <!-- Email -->
      <div class="space-y-1.5">
        <label
          for="contact-email"
          class="flex items-baseline justify-between gap-2 text-sm font-medium text-slate-900 dark:text-slate-100"
        >
          <span class="flex items-baseline gap-1">
            <span>Email address</span>
            <span class="text-rose-700 dark:text-rose-400">
              *Required
            </span>
          </span>

          <span class="text-sm font-normal text-slate-500 dark:text-slate-300">
            Example: you@example.com
          </span>
        </label>

        <input
          id="contact-email"
          ref="emailInput"
          name="email"
          type="email"
          required
          autocomplete="email"
          :aria-invalid="!!errors.email"
          :aria-describedby="errors.email ? 'contact-email-error' : undefined"
          class="block w-full rounded-2xl border bg-slate-50/60 px-3 py-2 text-sm text-slate-900 placeholder:text-slate-400 shadow-sm focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-brand-teal focus-visible:ring-offset-2 focus-visible:ring-offset-white dark:bg-slate-900/60 dark:text-slate-50 dark:placeholder:text-slate-500 dark:focus-visible:ring-brand-accent dark:focus-visible:ring-offset-slate-950"
          :class="errors.email
            ? 'border-rose-600 dark:border-rose-500'
            : 'border-slate-200 dark:border-slate-700'"
          placeholder="you@example.com"
          :value="contact.email"
          @input="updateField('email', ($event.target as HTMLInputElement).value)"
        />

        <p
          v-if="errors.email"
          id="contact-email-error"
          class="text-xs text-rose-700 dark:text-rose-400"
          role="alert"
        >
          {{ errors.email }}
        </p>
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
        :value="contact.note"
        @input="updateField('note', ($event.target as HTMLTextAreaElement).value)"
      />
    </fieldset>

    <div class="mt-4 flex flex-col sm:flex-row sm:justify-between items-stretch gap-4">
      <Button
        type="button"
        variant="secondary"
        @click="emit('back')"
      >
        ← Back
      </Button>
      <Button
        type="button"
        variant="primary"
        :disabled="!isFormValid"
        :aria-disabled="!isFormValid"
        @click="handleNext"
      >
        Review and submit →
      </Button>
    </div>
  </div>
</template>
