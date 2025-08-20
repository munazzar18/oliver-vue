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

      <label v-if="['radio', 'checkbox'].includes(type)" :for="item.id" v-bind="resolvedAttrs.labelAttrs">
        <span v-if="type === 'checkbox'"
          :class="item.badge ? 'text-sm cursor-pointer flex items-baseline text-text dark:text-dark-text' : 'text-sm cursor-pointer text-text dark:text-dark-text'">
          <span>
            {{ item.label }}
          </span>
          <div v-if="item.badge"
            class="h-[1.125rem] flex justify-center items-center px-1.5 rounded-dash-tag bg-dash-published dark:bg-dark-dash-published ml-2">
            <span class="text-xs font-medium">{{ item.badgeText }}</span>
          </div>
        </span>
        <span v-else>
          {{ item.label }}
        </span>
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
  badgeText: { type: String, default: '' },
  badge: { type: Boolean, default: false },


  radioOptions: {
    type: Array as () => { label: string; value: string | boolean, badge?: boolean, badgeText?: string }[],
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
      addClass: props.type === 'checkbox' ? 'flex flex-col gap-1.5' :
        props.type === 'radio' ? 'flex flex-col gap-[0.375rem] flex-1 self-stretch' :
          'flex flex-col gap-2',
      addAttributes: { 'data-wrapper': 'wrapper1' }
    },
    {
      targetAttribute: 'wrapper2',
      addClass: props.type === 'checkbox' ? 'flex flex-col gap-4' :
        props.type === 'radio' ? 'flex flex-col gap-2'
          : 'flex justify-between items-center w-full',
      addAttributes: { 'data-wrapper': 'wrapper2' }
    },
    {
      targetAttribute: 'wrapper3',
      addClass: props.type === 'checkbox' ? 'flex items-start gap-2'
        : props.type === 'radio' ? 'flex items-center gap-2 relative'
          : 'rounded-[0.625rem] border border-border dark:border-border-dark bg-input dark:bg-input-dark min-h-10 gap-2.5 pt-3 pb-3 px-2.5 flex justify-center items-center self-stretch',
      addAttributes: { 'data-wrapper': 'wrapper3' }
    },
  ],
  elm: {
    addClass: props.type === 'checkbox' ?
      'w-4 min-w-4 h-4 cursor-pointer accent-primary dark:accent-dark-primary' :
      props.type === 'radio' ?
        'relative pl-8 cursor-pointer text-[0.938rem] font-medium leading-6 text-dash-gray-900 dark:text-dark-dash-text peer-checked:before:bg-dash-gray-900 dark:peer-checked:before:bg-dark-dash-text peer-checked:after:content-[""] peer-checked:after:w-[0.375rem] peer-checked:after:h-[0.375rem] peer-checked:after:bg-dash-success dark:peer-checked:after:bg-dark-dash-success peer-checked:after:rounded-full peer-checked:after:absolute peer-checked:after:left-[0.4375rem] peer-checked:after:top-1/2 peer-checked:after:-translate-y-1/2 before:content-[""] before:w-5 before:h-5 before:rounded-full before:border before:border-dash-gray-300 dark:before:border-dark-dash-border before:bg-white dark:before:bg-dark-surface before:absolute before:left-0 before:top-1/2 before:-translate-y-1/2 before:box-border' :
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
        'block text-sm font-medium text-text dark:text-dark-text' :
        props.type === 'radio' ?
          'block text-sm font-medium text-dash-gray-700 dark:text-dark-dash-text' :
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
  if ((props.type === "radio" || props.type === "checkbox") && props.radioOptions?.length) {
    return props.radioOptions.map(option => ({
      label: option.label,
      value: option.value,
      badge: option.badge,
      badgeText: option.badgeText,
      id: `${props.addId || resolvedAttrs.value.inputAttrs.id}-${option.value}`
    }));
  }

  return [
    {
      label: props.labelText || "",
      value: props.modelValue,
      badge: props.badge,
      badgeText: props.badgeText,
      id: props.addId || resolvedAttrs.value.inputAttrs.id
    }
  ];
});
</script>
