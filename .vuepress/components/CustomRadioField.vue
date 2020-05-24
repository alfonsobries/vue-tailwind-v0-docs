<template>
  <input-demo>
    <div
      v-for="(option, index) in options"
      class="flex py-1"
    >
      <t-radio
        ref="input"
        :key="option.value"
        :id="`${id}-${index}`"
        v-model="model"
        :disabled="disabled"
        :name="name"
        :value="option.value"
        :status="status"
        class="custom-radio"
      />
      <label class="ml-3" :for="`${id}-${index}`">{{ option.text }}</label>
    </div>

    <template slot="controls">
      <disabled-control v-model="disabled" />

      <status-control v-model="status" />

      <p class="flex items-center mt-2">
        <label 
          for="rows" 
          class="mr-2">
          Add option:
        </label>
        <t-input
          id="newOption"
          v-model.number="newOption"
          name="newOption"
          default-class="p-1 border text-sm"
          @keyup.enter="addNewOption"
        />
      </p>
    </template>

    <template slot="value">
      <p>Current value: <pre class="text-white">{{ model }}</pre></p>
    </template>

  </input-demo>
</template>

<script>
import getRenderedClass from '../mixins/getRenderedClass'


export default {
  name: 'CustomRadioField',

  mixins: [getRenderedClass],

  data () {
    return {
      model: 'Option 2',
      id: 'custom-radio-field',
      name: 'custom-radio-field',
      options: [
        { value: 'Option 1', text: 'Option 1' },
        { value: 'Option 2', text: 'Option 2' },
        { value: 'Option 3', text: 'Option 3' },
        { value: 'Option 4', text: 'Option 4' },
        { value: 'Option 5', text: 'Option 5' },
      ],
      newOption: '',
    }
  },

  methods: {
    addNewOption() {
      if (!this.newOption) {
        return
      }
      const option = { value: this.newOption, text: this.newOption }
      this.options.push(option)
      this.model = option.value
      this.newOption = ''
    },
  }
}
</script>

<style>
input.custom-radio:checked,
input.custom-radio:not(:checked) {
  @apply absolute;
  left: -9999px;
}
input.custom-radio:checked + label,
input.custom-radio:not(:checked) + label
{
  @apply relative pl-8 cursor-pointer leading-normal inline-block;
}
input.custom-radio:checked + label:before,
input.custom-radio:not(:checked) + label:before {
  @apply rounded-full absolute border border-gray-400 top-0 left-0 w-6 h-6 bg-white;
  content: '';
}
input.custom-radio:checked + label:after,
input.custom-radio:not(:checked) + label:after {
  @apply w-4 h-4 top-0 left-0 m-1 bg-blue-500 absolute rounded-full;
  content: '';
  transition: all 0.2s ease;
}

input.custom-radio:not(:checked) + label:after {
  @apply opacity-0;
  transform: scale(0);
}
input.custom-radio:checked + label:after {
  @apply opacity-100;
  transform: scale(1);
}

input.custom-radio.t-radio-disabled + label:after {
  @apply opacity-50;
}

/** By status */
input.custom-radio.t-radio-status-error:checked + label:after,
input.custom-radio.t-radio-status-error:not(:checked) + label:after {
  @apply bg-red-500
}
input.custom-radio.t-radio-status-success:checked + label:after,
input.custom-radio.t-radio-status-success:not(:checked) + label:after {
   @apply bg-green-500
}
input.custom-radio.t-radio-status-warning:checked + label:after,
input.custom-radio.t-radio-status-warning:not(:checked) + label:after {
  @apply bg-yellow-500
}
</style>
