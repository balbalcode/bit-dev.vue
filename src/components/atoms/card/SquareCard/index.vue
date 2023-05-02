<script>
export default {
  name: 'SquareCard',
  props: {
    id: { default: `element-${Math.random()}` },
    padding: { type: String, default: '3' },
    col: { default: 12 },
    responsive: { default: 12 },
    has_shadow: { type: Boolean, default: true },
    has_border: { type: Boolean, default: false },
    has_default_padding: { type: Boolean, default: true },
    variant: { type: String, default: 'white' },
    additional_class: { type: String, default: '' },
  },
}
</script>
<template>
  <b-col
    :cols="responsive"
    :md="col"
    :id="id"
    :class="`bg-${variant} ${has_border ? 'border' : ''} ${
      has_default_padding ? 'p-3' : ''
    } rounded ${additional_class} ${has_shadow ? 'shadows' : ''}`"
  >
    <slot />
  </b-col>
</template>
