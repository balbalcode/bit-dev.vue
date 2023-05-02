<script>
export default {
  name: 'PlainLabel',
  props: {
    id: { default: '' },
    value: { type: [String, Number], default: '' },
    variant: { type: String, default: 'dark' },
    is_bold: { type: Boolean, default: false },
    size: { default: '13' },
    is_italic: { type: Boolean, default: false },
    additional_class: { type: String, default: '' },
  },
}
</script>
<template>
  <label
    :id="id"
    :ref="id"
    :class="`${is_bold ? `font-weight-bold` : ``} ${
      is_italic ? `font-italic` : ``
    } ${size ? 'font-size-' + size : ``} text-${variant} ${additional_class}`"
  >
    <span v-html="value"></span> {{ !value || value === '' ? '-' : '' }}
  </label>
</template>
