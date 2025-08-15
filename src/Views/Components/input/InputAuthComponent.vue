<template>
  <div v-bind="resolvedAttrs.wrapperAttrs.outerWrapper || {}">
    <div v-bind="resolvedAttrs.wrapperAttrs.wrapper1">
      <!-- Label -->
      <div v-bind="resolvedAttrs.wrapperAttrs.labelWrapper">
        <label v-bind="resolvedAttrs.labelAttrs" :for="addId || resolvedAttrs.inputAttrs.id"
          class="text-sm leading-6 tracking-[0.009rem] text-text dark:text-text-dark">
          {{ labelText }}
        </label>
        <span v-if="showRequired" v-bind="resolvedAttrs.requiredAttrs"
          class="text-[0.625rem] leading-6 text-right italic text-text dark:text-text-dark">
          Required
        </span>
      </div>

      <!-- Input -->
      <div v-bind="resolvedAttrs.wrapperAttrs.inputWrapper">
        <input v-bind="resolvedAttrs.inputAttrs"
          class="w-full font-medium text-base text-text dark:text-text-dark bg-transparent outline-none leading-6 tracking-[0.01rem] placeholder:text-placeholder placeholder:font-light placeholder:text-base placeholder:leading-6 placeholder:tracking-[0.01rem] dark:placeholder:text-placeholder-dark"
          :id="addId || resolvedAttrs.inputAttrs.id"
          :type="isPasswordType ? (showPassword ? 'text' : 'password') : type" :value="modelValue"
          @input="handleInput" />

        <!-- Password eye toggle -->
        <div v-if="isPasswordType" v-bind="resolvedAttrs.wrapperAttrs.eyeWrapper"
          class="flex justify-center items-center cursor-pointer w-5 h-5" @click="togglePasswordVisibility">
          <EyeIcon v-if="showPassword" class="w-5 h-5 text-text dark:text-text-dark" />
          <EyeSlashIcon v-else class="w-5 h-5 text-text dark:text-text-dark" />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue'
import { EyeIcon, EyeSlashIcon } from '@heroicons/vue/24/outline'
import { resolveAllConfigs } from '../../../utils/componentRenderingUtils'

// Emits
const emit = defineEmits<{ 'update:modelValue': [value: string] }>()

// Local state for password visibility
const showPassword = ref(false)
const togglePasswordVisibility = () => { showPassword.value = !showPassword.value }

// Props
const props = defineProps({
  modelValue: [String, Number],
  version: { type: String, default: '' },

  // Input attributes
  addId: String,
  removeId: Boolean,
  addClass: String,
  removeClass: Boolean,
  addAttributes: Object,
  removeAttributes: { type: Array as () => string[], default: () => [] },

  // HTML input props
  type: { type: String, default: 'text' }, // email, password, text, etc.
  name: { type: String, default: '' },
  placeholder: { type: String, default: '' },
  required: { type: Boolean, default: false },
  autocomplete: { type: String, default: '' },

  // Label
  labelText: { type: String, default: '' },
  showRequired: { type: Boolean, default: true },

  // Wrapper overrides
  wrapperOverrides: { type: Array as () => any[], default: () => [] }
})

// Handle input
const handleInput = (event: Event) => {
  const target = event.target as HTMLInputElement
  emit('update:modelValue', target.value)
}

// Check if password type
const isPasswordType = computed(() => props.type === 'password')

// Config factory (shared for all input types)
const inputConfig = computed(() => {
  const wrappers = [
    { targetAttribute: 'outerWrapper', addClass: 'flex flex-col gap-4', addAttributes: { 'data-wrapper': 'outerWrapper' } },
    { targetAttribute: 'wrapper1', addClass: 'flex flex-col gap-2', addAttributes: { 'data-wrapper': 'wrapper1' } },
    { targetAttribute: 'labelWrapper', addClass: 'flex justify-between items-center w-full', addAttributes: { 'data-wrapper': 'labelWrapper' } },
    { targetAttribute: 'inputWrapper', addClass: 'rounded-[0.625rem] border border-border dark:border-border-dark bg-input dark:bg-input-dark min-h-10 gap-2.5 pt-3 pb-3 px-2.5 flex justify-center items-center self-stretch', addAttributes: { 'data-wrapper': 'inputWrapper' } }
  ]

  // Eye icon wrapper only for password
  if (isPasswordType.value) {
    wrappers.push({ targetAttribute: 'eyeWrapper', addClass: '', addAttributes: { 'data-wrapper': 'eyeWrapper' } })
  }

  return {
    wrappers,
    elm: {
      addId: props.addId || `auth-${props.type}`,
      addAttributes: {
        type: props.type
      }
    },
    additionalConfig: {
      label: {
        addAttributes: {
          for: props.addId || `auth-${props.type}`
        }
      },
      required: {
        addAttributes: {
          'data-required': String(props.required)
        }
      }
    }
  }
})

// Resolve attributes
const resolvedAttrs = computed(() => {
  const resolved = resolveAllConfigs(inputConfig.value, props.version, props)
  return {
    ...resolved,
    requiredAttrs: inputConfig.value.additionalConfig?.required || {}
  }
})
</script>
