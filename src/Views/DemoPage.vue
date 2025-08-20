<template>
  <div>
    <section class="bg-gray-50 p-8 shadow-md mx-4 flex flex-col gap-2 ">
      <h1 class="text-3xl font-bold mb-4">Auth Demo components</h1>
      <AuthDemo />
    </section>
    <section class="bg-gray-50 p-8 shadow-md mx-4 flex flex-col gap-2 ">
      <h1 class="text-3xl font-bold mb-4">Paragraph demo component</h1>
      <div>
        <Paragraph text="Hello, this is a styled paragraph component." font-size="text-lg" font-weight="font-semibold"
          font-color="text-gray-700 dark:text-gray-200" shadow="shadow-sm" :left-icon="PaperClipIcon" />
      </div>
      <div>
        <Paragraph text="This is another paragraph component." font-size="text-sm" font-weight="font-normal"
          font-color="text-blue-800 dark:text-gray-200" />
      </div>
    </section>
    <section class="bg-gray-50 p-8 shadow-md mx-4 flex flex-col gap-2 ">
      <h1 class="text-3xl font-bold mb-4">Heading demo component</h1>
      <div>
        <Heading text="Dashboard h1 heading" tag="h1" theme="defaultPrimaryHeading" />
      </div>
      <div>
        <Heading text="This is a h3 heading" tag="h3" />
      </div>
      <div>
        <Heading text="This is a h2 heading" tag="h2" theme="defaultSecondaryHeading" />
      </div>
      <div>
        <Heading text="This is a Section h1 heading" tag="h1" theme="sectionHeading" />
      </div>
    </section>
    <section class="bg-gray-50 p-8 shadow-md mx-4 flex flex-col gap-2 ">
      <h1 class="text-3xl font-bold mb-4">Button demo component</h1>
      <div><Button text="Save" variant="success" size="lg" /></div>
      <div><Button text="Delete" variant="danger" size="sm" /></div>
      <div><Button text="With Icon" :left-icon="PaperClipIcon" variant="secondary" size="md" /></div>
      <div><Button text="More Info" variant="outline" size="md" /></div>
    </section>

    <section class="bg-gray-50 p-8 shadow-md mx-4 flex flex-col gap-2 ">
      <h1 class="text-3xl font-bold mb-4">Default demo components</h1>
      <div>
        <h3 class="text-xl font-bold mb-4">Check Box</h3>
        <InputDefaultComponent id="checkbox" type="checkbox" show-label label-text="Agree to terms" :radio-options="[{ label: 'Show fan\'s username on your post', value: true }, { label: 'I have verified, preserved and currently hold all identification cards and documents required under Record Keeping Requirements, 18 U.S.C. 2257 and 28 C.F.R. 75, or applicable laws in any other jurisdictions, countries, and territories, for all individuals appearing in the Content.', value: true, },
        { label: 'Silver tier', value: true, badge: true, badgeText: 'Published Tier' }
        ]" />

      </div>
      <div>
        <h3 class="text-xl font-bold mb-4">Radio Box</h3>
        <InputDefaultComponent id="radiobox" type="radio" show-label label-text="Subscription Tier"
          :radio-options="[{ label: 'Choice A', value: 'A' }, { label: 'Choice B', value: 'B' }, { label: 'Choice C', value: 'C' }]" />
      </div>
    </section>
  </div>
</template>

<script setup lang="ts">
import AuthDemo from './AuthDemo.vue'
import Paragraph from './Components/Form/Paragraph.vue';
import Heading from './Components/Form/Heading.vue';
import Button from './Components/UI/ButtonComponent.vue';
import { PaperClipIcon } from '@heroicons/vue/24/outline';
import InputDefaultComponent from './Components/input/InputDefaultComponent.vue';
</script>

<style scoped></style>
