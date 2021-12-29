<template>
  <h6 class="text-gray-900 text-lg font-medium">
    {{ $t('settings.customization.invoices.defaultstatus') }}
  </h6>
  <p class="mt-1 text-sm text-gray-500">
    {{ $t('settings.customization.invoices.defaultstatus_description') }}
  </p>

  <BaseInputGroup required>
    <BaseRadio
      id="draft"
      v-model="settingsForm.defaultstatus_edits"
      :label="$t('settings.customization.invoices.draft')"
      size="sm"
      name="filter"
      value="draft"
      class="mt-2"
      @update:modelValue="submitForm"
    />

    <BaseRadio
      id="sent"
      v-model="settingsForm.defaultstatus_edits"
      :label="$t('settings.customization.invoices.sent')"
      size="sm"
      name="filter"
      value="sent"
      class="mt-2"
      @update:modelValue="submitForm"
    />
  </BaseInputGroup>
</template>

<script setup>
import { reactive, computed, ref, inject } from 'vue'
import { useCompanyStore } from '@/scripts/stores/company'
import { useI18n } from 'vue-i18n'
import { useGlobalStore } from '@/scripts/stores/global'

const { t, tm } = useI18n()
const companyStore = useCompanyStore()
const globalStore = useGlobalStore()
const utils = inject('utils')

const settingsForm = reactive({ defaultstatus_edits: null })

utils.mergeSettings(settingsForm, {
  ...companyStore.selectedCompanySettings,
})

const retrospectiveEditOptions = computed(() => {
  return globalStore.config.defaultstatus_edits.map((option) => {
    option.title = t(option.key)
    return option
  })
})

async function submitForm() {
  let data = {
    settings: {
      ...settingsForm,
    },
  }

  await companyStore.updateCompanySettings({
    data,
    message: 'settings.customization.invoices.invoice_settings_updated',
  })

  return true
}
</script>
