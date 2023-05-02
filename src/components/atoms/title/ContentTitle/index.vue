<script>
export default {
  props: {
    id: { default: '' },
    value: { default: '' },
    size: { default: 'md' },
    variant: { default: 'dark' },
    additional_class: { default: 'font-weight-bolder ' },
  },
}
</script>
<template>
  <div>
    <h4
      v-if="size === 'md'"
      :id="id"
      :class="`text-${variant} ${additional_class}`"
      >{{ value }}</h4
    >
    <h6
      v-if="size === 'sm'"
      :id="id"
      :class="`text-${variant} ${additional_class}`"
      >{{ value }}</h6
    >
  </div>
</template>
