<template>
  <div v-bind="resolvedAttrs.wrapperAttrs.wrapper1">
    <div v-bind="resolvedAttrs.wrapperAttrs.wrapper2">
      <label v-if="showLabel" v-bind="resolvedAttrs.labelAttrs">
        {{ labelText }}
      </label>
      <span v-if="requiredDisplay === '*'" class="text-red-500">*</span>
      <span v-else-if="requiredDisplay === 'italic-text'"
        class="text-[0.625rem] leading-6 text-right italic text-text dark:text-text-dark">
        Required
      </span>
    </div>
    <div v-bind="resolvedAttrs.wrapperAttrs.wrapper3" class="relative" v-for="item in inputItems" :key="item.id">

      <component v-if="leftIcon && type !== 'checkbox' && type !== 'radio'" :is="leftIcon"
        class="absolute left-2 top-[50px] transform -translate-y-1/2 pointer-events-none w-5 h-5 text-white" />

      <input v-bind="resolvedAttrs.inputAttrs" :id="item.id" :type="type" :value="item.value"
        :checked="type === 'checkbox' ? Boolean(modelValue) : modelValue === item.value" @input="$emit(
          'update:modelValue',
          type === 'checkbox'
            ? ($event.target as HTMLInputElement).checked
            : ($event.target as HTMLInputElement).value
        )" />

      <label v-if="type === 'radio'" :for="item.id" v-bind="resolvedAttrs.labelAttrs">
        {{ item.label }}
      </label>

      <component v-if="rightIcon && type !== 'checkbox' && type !== 'radio'" :is="rightIcon"
        class="absolute right-2 mr-1 top-[25px] transform -translate-y-1/2 pointer-events-none w-5 h-5 text-white" />

      <p v-if="description" v-bind="resolvedAttrs.descriptionAttrs" class="mt-1 text-sm text-white/80">
        {{ description }}
      </p>
    </div>

  </div>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue'
import { resolveAllConfigs } from '../../../utils/componentRenderingUtils'

const props = defineProps({
  modelValue: [String, Number, Boolean],
  version: { type: String, default: '' },


  radioOptions: {
    type: Array as () => { label: string; value: string }[],
    required: false,
    default: () => []
  },


  addId: String,
  removeId: Boolean,
  addClass: String,
  removeClass: Boolean,
  addAttributes: Object,
  removeAttributes: { type: Array as () => string[], default: () => [] },


  name: String,
  type: { type: String, default: 'text' },
  placeholder: String,
  required: Boolean,
  autocomplete: String,


  showLabel: Boolean,
  labelText: { type: String, default: 'Label' },
  requiredDisplay: { type: String, default: '' }, // "*" or "italic-text"


  description: String,


  leftIcon: [String, Object, Function],
  rightIcon: [String, Object, Function],


  radioValue: String,


  wrapperOverrides: { type: Array as () => any[], default: () => [] }
})


const inputConfig = {
  wrappers: [
    {
      targetAttribute: 'wrapper1',
      addClass: props.type === 'checkbox' ? 'flex justify-start items-center gap-2 flex-shrink-0' :
        'flex flex-col gap-2',
      addAttributes: { 'data-wrapper': 'wrapper1' }
    },
    {
      targetAttribute: 'wrapper2',
      addClass: props.type === 'checkbox' ? '' :
        props.type === 'radio' ? 'flex items-center gap-2'
          : 'flex justify-between items-center w-full',
      addAttributes: { 'data-wrapper': 'wrapper2' }
    },
    {
      targetAttribute: 'wrapper3',
      addClass: props.type === 'checkbox' ? ''
        : props.type === 'radio' ? 'flex items-center gap-2'
          : 'rounded-[0.625rem] border border-border dark:border-border-dark bg-input dark:bg-input-dark min-h-10 gap-2.5 pt-3 pb-3 px-2.5 flex justify-center items-center self-stretch',
      addAttributes: { 'data-wrapper': 'wrapper3' }
    },
  ],
  elm: {
    addClass: props.type === 'checkbox' ?
      'm-0 border border-checkboxBorder dark:border-checkboxBorder-dark [appearance:none] w-[0.75rem] h-[0.75rem] rounded-[2px] bg-transparent relative cursor-pointer checked:bg-checkbox checked:border-checkbox dark:checked:bg-checkbox-dark dark:checked:border-checkbox-dark checked:[&::after]:content-[\'\'] checked:[&::after]:absolute checked:[&::after]:left-[0.2rem] checked:[&::after]:w-[0.25rem] checked:[&::after]:h-[0.5rem] checked:[&::after]:border checked:[&::after]:border-solid checked:[&::after]:border-white checked:[&::after]:border-r-[2px] checked:[&::after]:border-b-[2px] checked:[&::after]:border-t-0 checked:[&::after]:border-l-0 checked:[&::after]:rotate-45 dark:checked:[&::after]:border-[#303436]' :
      props.type === 'radio' ?
        'w-4 h-4 cursor-pointer accent-primary dark:accent-dark-primary' :
        'w-full font-medium text-base text-text dark:text-text-dark bg-transparent outline-none leading-6 tracking-[0.01rem] placeholder:text-placeholder placeholder:font-light placeholder:text-base placeholder:leading-6 placeholder:tracking-[0.01rem] dark:placeholder:text-placeholder-dark' +
        (props.leftIcon ? 'pl-10' : 'pl-3') + ' ' +
        (props.rightIcon ? 'pr-10' : 'pr-3'),
    addAttributes: {
      type: props.type
    }
  },
  additionalConfig: {
    label: {
      addClass: props.type === 'checkbox' ?
        'text-[0.875rem] leading-6 text-text dark:text-text-dark cursor-pointer' :
        props.type === 'radio' ?
          'text-sm cursor-pointer text-text dark:text-dark-text' :
          'text-sm leading-6 tracking-[0.009rem] text-text dark:text-text-dark',
      addAttributes: {
        for: 'input-id'
      }
    },
    description: {
      addClass: 'text-sm text-white/80',
      addAttributes: {
        'data-description': 'true'
      }
    }
  }
}

const resolvedAttrs = computed(() =>
  resolveAllConfigs(inputConfig, props.version, props)
)

const inputItems = computed(() => {
  if (props.type === "radio" && props.radioOptions?.length) {
    return props.radioOptions.map(option => ({
      label: option.label,
      value: option.value,
      id: `${props.addId || resolvedAttrs.value.inputAttrs.id}-${option.value}`
    }));
  }

  return [
    {
      label: props.labelText || "",
      value: props.modelValue,
      id: props.addId || resolvedAttrs.value.inputAttrs.id
    }
  ];
});
</script>
