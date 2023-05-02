<script>
export default {
  name: 'PriceLabel',
  props: {
    id: { default: '' },
    value: { default: '' },
    variant: { default: 'dark' },
    is_bold: { default: false, type: Boolean },
    is_italic: { default: false, type: Boolean },
    has_label: { default: false, type: Boolean },
    size: { default: '' },
    additional_class: { default: '' },
  },
}
</script>
<template>
  <label
    :class="`${is_bold ? `font-weight-bold` : ``} ${
      is_italic ? `font-italic` : ``
    } ${size ? 'font-size-' + size : ``} text-${variant} ${additional_class}`"
    :id="id"
    :ref="id"
  >
    <span :id="`tag_label_${id}`" :ref="`tag_label_${id}`" v-if="has_label"
      >Rp
    </span>
    <span :id="`price_label_${id}`" :ref="`price_label_${id}`">
      {{ $utility.convertToRupiah(value) }}
    </span>
  </label>
</template>
