<template>
  <div class="bg-white text-text p-2 max-w-7xl mx-auto dark:bg-black">
    <div class="flex flex-col gap-10 dark:bg-black">
      <section class="flex flex-col gap-6">

        <section class="bg-gray-50 p-8 shadow-md mx-4 flex flex-col gap-2 ">
          <h1 class="text-3xl font-bold mb-4">Auth Demo components</h1>
          <AuthDemo />
        </section>
        <section class="bg-gray-50 p-8 shadow-md mx-4 flex flex-col gap-2 ">
          <h1 class="text-3xl font-bold mb-4">Paragraph demo component</h1>
          <div>
            <Paragraph text="Hello, this is a styled paragraph component." font-size="text-lg"
              font-weight="font-semibold" font-color="text-gray-700 dark:text-gray-200" shadow="shadow-sm"
              :left-icon="PaperClipIcon" />
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
          <h1 class="text-3xl font-bold mb-4">Default demo components in single Input</h1>
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

          <div class="w-3/4 mb-4">
            <h3 class="text-xl font-bold mb-4">Input Box</h3>
            <InputDefaultComponent id="input" type="text" placeholder="Enter your name" show-label v-model="name"
              label-text="Text input with description"
              description="Please enter your full name as it appears on your ID" />
          </div>

          <div class="w-3/4 mb-4">
            <InputDefaultComponent id="input_1" type="text" placeholder="This is a required field" show-label
              v-model="name" label-text="Required Input" requiredDisplay="*" />

          </div>

          <div class="w-3/4 mb-4">
            <InputDefaultComponent id="input_2" type="text" placeholder="Input with left icon" show-label v-model="name"
              label-text="Input with left Icon" :left-icon="EnvelopeIcon" />

          </div>

          <div class="w-3/4 mb-4">
            <InputDefaultComponent id="input_3" type="text" placeholder="Input with right icon" show-label
              v-model="name" label-text="Input with right Icon" :right-icon="EyeIcon" />
          </div>

          <div class="w-3/4 mb-4">
            <InputDefaultComponent id="input_4" type="text" placeholder="Enter discount code" show-label v-model="name"
              label-text="Input with Optional text" isOptional />
          </div>

          <div class="w-3/4 mb-4">
            <InputDefaultComponent id="input_5" type="number" placeholder="Enter Discount percentage" show-label
              v-model="name" label-text="Input with Right Span Text" rightSpan :rightSpanText="'% off'" />
          </div>

          <div class="w-3/4 mb-4">
            <InputDefaultComponent id="input_6" type="number" placeholder="Monthly Price" show-label leftSpan
              :leftSpanText="'USD$'" v-model="name" label-text="Input with Right Span Text & Left Span Text" rightSpan
              :rightSpanText="'/mo'" :rightSpanClass="'text-slate-500'" />
          </div>

          <div class="w-3/4 mb-4">
            <InputDefaultComponent id="input_7" type="textarea" placeholder="Enter multiline text" show-label
              v-model="name" label-text="Input with Type Text Area" description="30/200 characters" />
          </div>

          <div class="w-3/4 mb-4">
            <InputDefaultComponent id="input_8" type="textarea" placeholder="Enter multiline text" show-label
              richTextEditor v-model:content="name" label-text="Textarea with Quills library"
              description="30/200 characters" />
          </div>
        </section>

        <section class="bg-gray-50 p-8 shadow-md mx-4 flex flex-col gap-2 ">
          <h1 class="text-3xl font-bold mb-4">Dashboard components</h1>
          <div class="w-3/4 mb-4">
            <InputComponentDashbaord id="input_a" placeholder="Enter username" show-label v-model="name"
              label-text="Username"
              description="Usernames cannot be changed after your first month, but special circumstances may allow for exceptions. Please contact us if you need assistance with changing your username." />
          </div>

          <div class="w-3/4 mb-4">
            <InputComponentDashbaord id="input_b" placeholder="Enter username..." show-label v-model="name"
              label-text="Username"
              description="Usernames cannot be changed after your first month, but special circumstances may allow for exceptions. Please contact us if you need assistance with changing your username."
              requiredDisplay="required-text-error" showErrors :errors="[
                {
                  error: 'Must be between 3 and 100 characters long',
                  icon: InformationCircleIcon
                },
                {
                  error: 'Can contain any letters from a-z, any numbers from 0-9.',
                  icon: InformationCircleIcon
                },
                {
                  error: 'Cannot contain space.',
                  icon: InformationCircleIcon
                },
                {
                  error: 'Sorry, username \'minimay\' is taken',
                  icon: InformationCircleIcon
                }
              ]" />
          </div>


          <div class="w-3/4 mb-4">
            <InputComponentDashbaord id="input_b" placeholder="Enter username..." show-label v-model="name"
              label-text="Username"
              description="Usernames cannot be changed after your first month, but special circumstances may allow for exceptions. Please contact us if you need assistance with changing your username."
              onSuccess :success="[
                {
                  message: 'Must be between 3 and 100 characters long',
                  icon: CheckIcon
                },
                {
                  message: 'Can contain any letters from a-z, any numbers from 0-9.',
                  icon: CheckIcon
                },
                {
                  message: 'Cannot contain space.',
                  icon: CheckIcon
                },
                {
                  message: 'Good news! Username \'notminimay\' is available!',
                  icon: CheckIcon
                }
              ]" />
          </div>

          <div class="w-3/4 mb-4">
            <InputComponentDashbaord id="input_b" placeholder="Search with user id" show-label v-model="name"
              label-text="Co-performer (Optional)" :left-icon="MagnifyingGlassIcon" optionalLabel
              optionalLabelText=" If this media includes other performers, please tag them below." />

          </div>
        </section>

      </section>
    </div>
  </div>
</template>

<script setup lang="ts">
import AuthDemo from './AuthDemo.vue'
import Paragraph from './Components/Form/Paragraph.vue';
import Heading from './Components/Form/Heading.vue';
import Button from './Components/UI/ButtonComponent.vue';
import { PaperClipIcon, EnvelopeIcon, EyeIcon, InformationCircleIcon, CheckIcon, MagnifyingGlassIcon } from '@heroicons/vue/24/outline';
import InputDefaultComponent from './Components/input/InputDefaultComponent.vue';
import Input from './Components/Form/Input.vue';
import InputComponentDashbaord from './Components/input/InputComponentDashboard.vue';
import { ref } from 'vue';

const name = ref('')

</script>

<style scoped></style>
