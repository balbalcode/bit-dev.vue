<script>
export default {
  name: 'OverviewLabel',
  props: {
    id: { default: '' },
    value: { default: '' },
    variant: { default: 'dark' },
    is_bold: { default: true, type: Boolean },
    is_italic: { default: true, type: Boolean },
    has_space: { default: true, type: Boolean },
    limit_text: { default: '15' },
    additional_class: { default: '' },
    size: { default: 'md' },
  },
  methods: {
    renderFormattedText() {
      if (!this.value) return '-'
      if (this.value.length > parseInt(this.limit_text)) {
        return `${this.value.substr(0, this.limit_text)}${
          this.has_space ? ' ' : ''
        }...`
      } else return this.value
    },
  },
}
</script>
<template>
  <label
    :id="id"
    :ref="id"
    v-b-tooltip.hover.bottom="{
      title: value,
      disabled: limit_text > value.length,
    }"
    :class="`${is_bold ? `font-weight-bold` : ``} ${
      is_italic ? `font-italic` : ``
    } ${size ? 'font-size-' + size : ``} text-${variant} ${additional_class}`"
  >
    {{ renderFormattedText() }}
  </label>
</template>
