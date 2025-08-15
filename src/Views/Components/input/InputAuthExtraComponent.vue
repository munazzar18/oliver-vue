<template>
  <div v-bind="resolvedAttrs.wrapperAttrs.outerWrapper">
    <!-- Forgot password -->
    <RouterLink v-if="isForgotPassword" v-bind="resolvedAttrs.linkAttrs" :to="link"
      class="w-fit opacity-70 text-xs capitalize text-text dark:text-text-dark font-semibold">
      {{ forgotText }}
    </RouterLink>

    <!-- Remember me -->
    <div v-else v-bind="resolvedAttrs.wrapperAttrs.checkboxWrapper">
      <input v-bind="resolvedAttrs.checkboxInputAttrs"
        class="m-0 border border-checkboxBorder dark:border-checkboxBorder-dark [appearance:none] w-[0.75rem] h-[0.75rem] rounded-[2px] bg-transparent relative cursor-pointer checked:bg-checkbox checked:border-checkbox dark:checked:bg-checkbox-dark dark:checked:border-checkbox-dark checked:[&::after]:content-[''] checked:[&::after]:absolute checked:[&::after]:left-[0.2rem] checked:[&::after]:w-[0.25rem] checked:[&::after]:h-[0.5rem] checked:[&::after]:border checked:[&::after]:border-solid checked:[&::after]:border-white checked:[&::after]:border-r-[2px] checked:[&::after]:border-b-[2px] checked:[&::after]:border-t-0 checked:[&::after]:border-l-0 checked:[&::after]:rotate-45 dark:checked:[&::after]:border-[#303436]"
        :checked="modelValue" @change="toggleCheckbox" />
      <label v-bind="resolvedAttrs.checkboxLabelAttrs" :for="checkboxId"
        class="text-[0.875rem] leading-6 text-text dark:text-text-dark cursor-pointer">
        {{ checkboxLabel }}
      </label>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import { RouterLink } from 'vue-router'
import { resolveAllConfigs } from '../../../utils/componentRenderingUtils'

// Emits
const emit = defineEmits<{ 'update:modelValue': [value: boolean] }>()

// Props
const props = defineProps({
  type: { type: String as () => 'forget-password' | 'remember-me', required: true },
  version: { type: String, default: '' },

  // Forgot password props
  link: { type: String, default: '#' },
  forgotText: { type: String, default: 'Forgot Password?' },

  // Checkbox props
  modelValue: { type: Boolean, default: false },
  checkboxLabel: { type: String, default: 'Remember me' },
  checkboxId: { type: String, default: 'auth-remember-me' },
  checkboxName: { type: String, default: 'auth-remember-me' },

  // Attribute overrides
  addClass: String,
  removeClass: Boolean,
  addAttributes: Object,
  removeAttributes: { type: Array as () => string[], default: () => [] },

  // Wrapper overrides
  wrapperOverrides: { type: Array as () => any[], default: () => [] }
})

// Computed for type check
const isForgotPassword = computed(() => props.type === 'forget-password')

// Checkbox change
const toggleCheckbox = (e: Event) => {
  emit('update:modelValue', (e.target as HTMLInputElement).checked)
}

// Config factory
const extraConfig = computed(() => {
  const wrappers = []
  if (isForgotPassword.value) {
    wrappers.push({
      targetAttribute: 'outerWrapper',
      addClass: 'flex flex-col gap-4',
      addAttributes: { 'data-wrapper': 'outerWrapper' }
    })
  } else {
    wrappers.push(
      { targetAttribute: 'outerWrapper', addClass: 'flex flex-col gap-4', addAttributes: { 'data-wrapper': 'outerWrapper' } },
      { targetAttribute: 'checkboxWrapper', addClass: 'flex justify-start items-center gap-2 flex-shrink-0', addAttributes: { 'data-wrapper': 'checkboxWrapper' } }
    )
  }

  return {
    wrappers,
    elm: {},
    additionalConfig: {
      link: { addAttributes: {} },
      checkboxInput: {
        addId: props.checkboxId,
        addAttributes: {
          type: 'checkbox',
          name: props.checkboxName,
          id: props.checkboxId
        }
      },
      checkboxLabel: {
        addAttributes: { for: props.checkboxId }
      }
    }
  }
})

// Resolve attrs
const resolvedAttrs = computed(() => {
  const baseConfig = extraConfig.value
  const resolved = resolveAllConfigs(baseConfig, props.version, props)
  return {
    ...resolved,
    linkAttrs: baseConfig.additionalConfig.link || {},
    checkboxInputAttrs: baseConfig.additionalConfig.checkboxInput || {},
    checkboxLabelAttrs: baseConfig.additionalConfig.checkboxLabel || {}
  }
})
</script>
