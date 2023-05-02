<script>
import { BFormRadio, BFormRadioGroup } from 'bootstrap-vue'
export default {
  components: { BFormRadio, BFormRadioGroup },
  props: {
    id: { type: String, default: 'radio' },
    size: { type: String, default: 'md' },
    value: { type: [String, Object], default: '' },
    name: { type: String, default: '' },
    is_disabled: { type: Boolean },
    is_vertical: { type: Boolean },
    options: { type: Array, default: {} },
    additional_class: { type: String, default: '' },
  },
  computed: {
    local_value: {
      get: function () {
        return this.value
      },
      set: function (value) {
        this.$emit('input', value)
      },
    },
  },
}
</script>
<template>
  <b-form-radio-group
    v-model="local_value"
    :ref="id"
    :id="id"
    :name="name"
    :stacked="is_vertical"
    :disabled="is_disabled"
    :size="size"
  >
    <b-form-radio
      v-for="(list, index) in options"
      :value="list.value"
      :key="index"
      :id="`option-${id}-${index}`"
      :ref="`option-${id}-${index}`"
      :class="additional_class"
      :disabled="!!list.disabled"
    >
      {{ list.text }}
    </b-form-radio>
  </b-form-radio-group>
</template>
