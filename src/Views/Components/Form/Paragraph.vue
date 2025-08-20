<template>
  <div v-bind="resolvedAttrs.wrapperAttrs.wrapper1">
    <div v-bind="resolvedAttrs.wrapperAttrs.wrapper2">
      <!-- Optional Left Icon -->
      <component v-if="leftIcon" :is="leftIcon" class="mr-2 inline-block w-5 h-5" />

      <!-- Paragraph Text -->
      <p v-bind="resolvedAttrs.inputAttrs">
        {{ text }}
      </p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from "vue";
import { resolveAllConfigs } from "../../../utils/componentRenderingUtils";

const props = defineProps({
  text: { type: String, required: true },

  // optional icons
  leftIcon: [String, Object, Function],

  // override props (same structure as Input)
  addId: String,
  removeId: Boolean,
  addClass: String,
  removeClass: Boolean,
  addAttributes: Object,
  removeAttributes: { type: Array as () => string[], default: () => [] },

  version: { type: String, default: "" },
  wrapperOverrides: { type: Array as () => any[], default: () => [] },

  // style props
  fontSize: { type: String, default: "text-base" },
  fontWeight: { type: String, default: "font-normal" },
  fontColor: { type: String, default: "text-text dark:text-text-dark" },
  fontFamily: { type: String, default: "font-sans" },
  shadow: { type: String, default: "" },
});

const paragraphConfig = {
  wrappers: [
    {
      targetAttribute: "wrapper1",
      addClass: "flex flex-col w-full",
      addAttributes: { "data-wrapper": "wrapper1" },
    },
    {
      targetAttribute: "wrapper2",
      addClass: "flex items-start gap-2",
      addAttributes: { "data-wrapper": "wrapper2" },
    },
  ],
  elm: {
    addClass: [
      props.fontSize,
      props.fontWeight,
      props.fontColor,
      props.fontFamily,
      props.shadow,
    ]
      .filter(Boolean)
      .join(" "),
    addAttributes: {
      role: "paragraph",
    },
  },
};

const resolvedAttrs = computed(() =>
  resolveAllConfigs(paragraphConfig, props.version, props)
);
</script>
