<script>
export default {
  props: {
    id: { default: '' },
    size: { default: 2 },
    additional_class: { default: '' },
  },
}
</script>
<template>
  <div :class="`row ${additional_class}`">
    <div :class="`col-12`">
      <hr :id="id" :class="{ [`my-${size}`]: true }" />
    </div>
  </div>
</template>
