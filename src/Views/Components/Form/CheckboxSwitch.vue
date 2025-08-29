<template>
  <div v-bind="resolvedAttrs.wrapperAttrs.wrapper1">
    <label v-if="showWrapperLabel" class="block text-sm font-medium text-dash-gray-900 dark:text-dark-dash-text">{{
      wrapperLabel }}</label>
    <div v-bind="resolvedAttrs.wrapperAttrs.wrapper2">
      <label class="relative inline-block w-8 h-4">
        <input type="checkbox" v-bind="resolvedAttrs.inputAttrs" :id="id" :checked="modelValue"
          @change="$emit('update:modelValue', ($event.target as HTMLInputElement).checked)" />
        <span
          class="absolute cursor-pointer top-0 left-0 right-0 bottom-0 bg-dash-gray-400 dark:bg-dash-gray-600 transition-all duration-100 ease-in-out rounded-[0.75rem] peer-checked:bg-dash-gray-900 dark:peer-checked:bg-dark-dash-text"></span>
        <span
          class="absolute w-3 h-3 top-1/2 left-[0.125rem] bg-white dark:bg-dark-surface transition-all duration-100 ease-in-out rounded-full shadow-dash-toggle dark:shadow-dark-dash-toggle transform -translate-y-1/2 peer-checked:translate-x-[1rem]"></span>
      </label>
      <label v-bind="resolvedAttrs.labelAttrs">{{ label }}
      </label>
    </div>
  </div>
</template>

<script setup lang="ts">
import { resolveAllConfigs } from "../../../utils/componentRenderingUtils"
import { computed } from "vue"

const props = defineProps<{
  modelValue: boolean
  label?: string
  id?: string
  showWrapperLabel?: boolean
  wrapperLabel?: string

  version: string
  addId?: string
  removeId?: boolean
  addClass?: string
  removeClass?: boolean
  addAttributes?: object
  removeAttributes?: string[]
  wrapperOverrides?: any[]
}>()

const inputConfig = {
  wrappers: [
    {
      targetAttribute: 'wrapper1',
      addClass: props.version === 'dashboard' ? 'flex flex-col gap-[0.375rem] flex-1 self-stretch' :
        props.version === 'auth' ? 'flex flex-col gap-[0.375rem] flex-1 self-stretch' :
          'flex flex-col gap-1.5',
      addAttributes: { 'data-wrapper': 'wrapper1' }
    },
    {
      targetAttribute: 'wrapper2',
      addClass: props.version === 'dashboard' ? 'flex items-center gap-2' :
        props.version === 'auth' ? 'flex flex-col gap-2' : 'flex gap-2',
      addAttributes: { 'data-wrapper': 'wrapper2' }
    },
  ],
  elm: {
    addClass: props.version === 'dashboard' ?
      'opacity-0 w-0 h-0 peer' :
      props.version === 'auth' ? '' : '',
    addAttributes: {
      type: props.version
    }
  },
  additionalConfig: {
    label: {
      addClass: props.version === 'dashboard' ?
        'text-base font-medium text-dash-gray-900 dark:text-dark-dash-text cursor-pointer' :
        props.version === 'auth' ?
          'block text-sm font-medium text-dash-gray-700 dark:text-dark-dash-text' :
          'block text-sm font-medium text-text dark:text-dark-text',
      addAttributes: {
        for: 'input-id'
      }
    },
    description: {
      addClass: 'text-xs text-slate-500 dark:text-dark-dash-text',
      addAttributes: {
        'data-description': 'true'
      }
    }
  }
}

const resolvedAttrs = computed(() =>
  resolveAllConfigs(inputConfig, props.version, props)
)


const emit = defineEmits<{
  'update:modelValue': [value: boolean]
}>()

const toggle = () => {
  emit('update:modelValue', !props.modelValue)
}
</script>
