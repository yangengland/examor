<template>
  <v-container class="main_width">
    <h2>{{ $t('title.addNote') }}</h2>
    <h5 class="text-medium-emphasis">{{ $t('subTitle.addNote') }}</h5>

    <v-divider class="mt-8"></v-divider>

    <form class="py-6">
      <v-text-field
        v-model="formData.noteName"
        class="mt-3"
        variant="outlined"
        density="compact"
        :label="$t('label.noteName')"
      />

      <v-select
        v-model="formData.noteType"
        class="mt-4"
        variant="outlined"
        density="compact"
        item-title="label"
        item-value="value"
        :label="$t('label.selectNoteType')"
        :items="noteTypeOptions"
        @update:model-value="handleSelectChange"
      />

      <t-config-provider :global-config="locale === 'en' ? enConfig : cnConfig">
        <t-upload
          v-show="formData.noteType === 'files'"
          v-model="formData.files"
          class="mt-1 mb-7"
          placeholder=""
          theme="file-flow"
          multiple
          :autoUpload="false"
        />
      </t-config-provider>

      <v-text-field
        v-show="formData.noteType === 'notion'"
        v-model="formData.notion"
        class="mt-4"
        variant="outlined"
        density="compact"
        :label="$t('label.notionDataBaseID')"
      />

      <div class="mt-4 d-flex justify-end">
        <v-btn color="primary" elevation="0" :block="true" :disabled="disabled">
          {{ $t('button.submit') }}
        </v-btn>
      </div>
    </form>
  </v-container>
</template>

<script lang="ts">
export default {
  name: 'AddNote',
}
</script>

<script setup lang="ts">
import { reactive, computed } from 'vue'
import { useI18n } from 'vue-i18n'

import enConfig from 'tdesign-vue-next/es/locale/en_US'
import cnConfig from 'tdesign-vue-next/es/locale/zh_CN'

const { t, locale } = useI18n()

const noteTypeOptions = computed(() => [
  {
    label: t('option.localFiles'),
    value: 'files',
  },
  {
    label: t('option.notion'),
    value: 'notion',
  },
])

const formData = reactive<{
  noteName: string
  namespace: string
  noteType: 'files' | 'notion' | null
  files: any[]
  notion: string
}>({
  noteName: '',
  namespace: '',
  noteType: null,
  files: [],
  notion: '',
})

const disabled = computed(() => {
  if (!formData.noteName) return true
  if (!formData.namespace) return true
  if (!formData.noteType) return true
  if (!formData.files.length && formData.noteType === 'files') return true
  if (!formData.notion && formData.noteType === 'notion') return true

  return false
})

const handleSelectChange = () => {
  formData.files = []
  formData.notion = ''
}
</script>

<style scoped lang="scss">
:deep(.v-field__field) {
  height: 38px;
  font-size: 14px;

  .v-field__input {
    padding-top: 5px !important;
  }
}

:deep(.t-upload__flow-bottom) {
  display: none;
}

:deep(.t-upload__flow-card-area) {
  border-radius: 10px;
  border-width: 2px;

  .t-upload__flow-empty {
    user-select: none !important;
  }
}
</style>
