<script>
import { BFormCheckboxGroup, BFormCheckbox } from 'bootstrap-vue'
export default {
  components: { BFormCheckboxGroup, BFormCheckbox },
  props: {
    id: { default: '' },
    value: { default: () => [] },
    size: { default: 'md' },
    options: { default: () => [] },
    is_vertical: { default: false },
    is_indeterminate: { default: false },
    is_disabled: { default: false },
    additional_class: { default: '' },
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
  <b-form-checkbox-group
    :id="id"
    :ref="id"
    :class="additional_class"
    v-model="local_value"
    :size="size"
    :indeterminate="is_indeterminate"
    :disabled="is_disabled"
    :stacked="is_vertical"
  >
    <b-form-checkbox
      v-for="(option, index) in options"
      :key="index"
      :value="option.value"
      :disabled="option.is_disabled"
      :id="`${id}-option-${index}`"
      :ref="`${id}-option-${index}`"
      :indeterminate.sync="is_indeterminate"
      :checked="option.checked"
    >
      {{ option.text }}
    </b-form-checkbox>
  </b-form-checkbox-group>
</template>
