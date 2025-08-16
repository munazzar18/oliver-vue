<template>
  <div v-bind="resolvedAttrs.wrapperAttrs.outerWrapper || {}">
    <div v-bind="resolvedAttrs.wrapperAttrs.wrapper1">
      <!-- ========== Label (for text/email/password/radio groups) ========== -->
      <div v-if="!isCheckboxType && !isRadioType" v-bind="resolvedAttrs.wrapperAttrs.labelWrapper">
        <label v-bind="resolvedAttrs.labelAttrs" :for="addId || resolvedAttrs.inputAttrs.id"
          class="text-sm leading-6 tracking-[0.009rem] text-text dark:text-text-dark">
          {{ labelText }}
        </label>
        <span v-if="showRequired" v-bind="resolvedAttrs.requiredAttrs"
          class="text-[0.625rem] leading-6 text-right italic text-text dark:text-text-dark">
          Required
        </span>
      </div>

      <!-- ========== Normal Input (text/email/password) ========== -->
      <div v-if="isTextLikeType" v-bind="resolvedAttrs.wrapperAttrs.inputWrapper">
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

      <!-- ========== Checkbox ========== -->
      <div v-else-if="isCheckboxType" class="flex justify-start items-center gap-2 flex-shrink-0">
        <input type="checkbox" :id="addId || 'auth-checkbox'" :name="name"
          class="m-0 border border-checkboxBorder dark:border-checkboxBorder-dark [appearance:none] w-[0.75rem] h-[0.75rem] rounded-[2px] bg-transparent relative cursor-pointer checked:bg-checkbox checked:border-checkbox dark:checked:bg-checkbox-dark dark:checked:border-checkbox-dark checked:[&::after]:content-[''] checked:[&::after]:absolute checked:[&::after]:left-[0.2rem] checked:[&::after]:w-[0.25rem] checked:[&::after]:h-[0.5rem] checked:[&::after]:border checked:[&::after]:border-solid checked:[&::after]:border-white checked:[&::after]:border-r-[2px] checked:[&::after]:border-b-[2px] checked:[&::after]:border-t-0 checked:[&::after]:border-l-0 checked:[&::after]:rotate-45 dark:checked:[&::after]:border-[#303436]"
          :checked="Boolean(modelValue)" @change="handleCheckbox" />
        <label :for="addId || 'auth-checkbox'"
          class="text-[0.875rem] leading-6 text-text dark:text-text-dark cursor-pointer">
          {{ labelText }}
        </label>
      </div>

      <!-- ========== Radio Group ========== -->
      <div v-else-if="isRadioType" class="flex flex-col gap-1.5">
        <label class="block text-sm font-medium text-text dark:text-dark-text">
          {{ labelText }}
        </label>
        <div class="flex flex-col gap-2">
          <div v-for="(option, idx) in radioOptions" :key="idx" class="flex items-center gap-2">
            <input type="radio" :id="`${name}-${idx}`" :name="name"
              class="w-4 h-4 cursor-pointer accent-primary dark:accent-dark-primary" :value="option.value"
              :checked="modelValue === option.value" @change="handleRadio(option.value)" />
            <label :for="`${name}-${idx}`" class="text-sm cursor-pointer text-text dark:text-dark-text">
              {{ option.label }}
            </label>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, ref } from "vue";
import { EyeIcon, EyeSlashIcon } from "@heroicons/vue/24/outline";
import { resolveAllConfigs } from "../../../utils/componentRenderingUtils";

// Emits
const emit = defineEmits<{ "update:modelValue": [value: any] }>();

// Local state for password visibility
const showPassword = ref(false);
const togglePasswordVisibility = () => {
  showPassword.value = !showPassword.value;
};

// Props
const props = defineProps({
  modelValue: [String, Number, Boolean],
  version: { type: String, default: "" },

  // Input attributes
  addId: String,
  removeId: Boolean,
  addClass: String,
  removeClass: Boolean,
  addAttributes: Object,
  removeAttributes: { type: Array as () => string[], default: () => [] },

  // HTML input props
  type: { type: String, default: "text" }, // email, password, text, checkbox, radio
  name: { type: String, default: "" },
  placeholder: { type: String, default: "" },
  required: { type: Boolean, default: false },
  autocomplete: { type: String, default: "" },

  // Label
  labelText: { type: String, default: "" },
  showRequired: { type: Boolean, default: true },

  // Radio Options
  radioOptions: {
    type: Array as () => { label: string; value: string }[],
    default: () => [],
  },

  // Wrapper overrides
  wrapperOverrides: { type: Array as () => any[], default: () => [] },
});

// Handlers
const handleInput = (event: Event) => {
  const target = event.target as HTMLInputElement;
  emit("update:modelValue", target.value);
};
const handleCheckbox = (event: Event) => {
  const target = event.target as HTMLInputElement;
  emit("update:modelValue", target.checked);
};
const handleRadio = (value: string) => {
  emit("update:modelValue", value);
};

// Computed type checkers
const isPasswordType = computed(() => props.type === "password");
const isCheckboxType = computed(() => props.type === "checkbox");
const isRadioType = computed(() => props.type === "radio");
const isTextLikeType = computed(
  () => !isCheckboxType.value && !isRadioType.value
);

// Config factory (for text/email/password)
const inputConfig = computed(() => {
  const wrappers = [
    {
      targetAttribute: "outerWrapper",
      addClass: "flex flex-col gap-4",
      addAttributes: { "data-wrapper": "outerWrapper" },
    },
    {
      targetAttribute: "wrapper1",
      addClass: "flex flex-col gap-2",
      addAttributes: { "data-wrapper": "wrapper1" },
    },
    {
      targetAttribute: "labelWrapper",
      addClass: "flex justify-between items-center w-full",
      addAttributes: { "data-wrapper": "labelWrapper" },
    },
    {
      targetAttribute: "inputWrapper",
      addClass:
        "rounded-[0.625rem] border border-border dark:border-border-dark bg-input dark:bg-input-dark min-h-10 gap-2.5 pt-3 pb-3 px-2.5 flex justify-center items-center self-stretch",
      addAttributes: { "data-wrapper": "inputWrapper" },
    },
  ];

  if (isPasswordType.value) {
    wrappers.push({
      targetAttribute: "eyeWrapper",
      addClass: "",
      addAttributes: { "data-wrapper": "eyeWrapper" },
    });
  }

  return {
    wrappers,
    elm: {
      addId: props.addId || `auth-${props.type}`,
      addAttributes: {
        type: props.type,
      },
    },
    additionalConfig: {
      label: {
        addAttributes: {
          for: props.addId || `auth-${props.type}`,
        },
      },
      required: {
        addAttributes: {
          "data-required": String(props.required),
        },
      },
    },
  };
});

// Resolve attributes
const resolvedAttrs = computed(() => {
  const resolved = resolveAllConfigs(inputConfig.value, props.version, props);
  return {
    ...resolved,
    requiredAttrs: inputConfig.value.additionalConfig?.required || {},
  };
});
</script>
