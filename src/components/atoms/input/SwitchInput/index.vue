<script>
import { BFormCheckbox } from 'bootstrap-vue'
export default {
  components: { BFormCheckbox },
  props: {
    id: { type: String, default: '' },
    variant: { type: String, default: 'primary' },
    is_disabled: { type: Boolean },
    size: { type: String, default: 'md' },
    value: { type: Boolean, default: true },
    checked_value: { type: Boolean, default: true },
    unchecked_value: { type: Boolean },
    checked_text: { type: String, default: 'Ya' },
    unchecked_text: { type: String, default: 'Tidak' },
    has_text: { type: Boolean },
    text_size: { type: String, default: '14' },
    text_variant: { type: String, default: 'dark' },
    additional_class_text: { type: String, default: '' },
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
  methods: {
    getCaptionText(value) {
      let text = ''
      if (value === this.checked_value) text = this.checked_text
      else if (value === this.unchecked_value) text = this.unchecked_text
      return text
    },
  },
}
</script>
<template>
  <b-form-checkbox
    :id="id"
    :ref="id"
    v-model="local_value"
    :unchecked-value="unchecked_value"
    :value="checked_value"
    :size="size"
    :disabled="is_disabled"
    :class="additional_class"
    switch
  >
    <strong
      v-if="has_text"
      :id="`label-${id}`"
      :class="`font-size-${text_size} text-${text_variant} ${additional_class_text}`"
    >
      {{ getCaptionText(local_value) }}
    </strong>
  </b-form-checkbox>
</template>
