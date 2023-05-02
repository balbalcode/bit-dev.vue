<script>
export default {
  props: {
    id: { default: '' },
    value: { default: '' },
    variant: { default: 'dark' },
  },
}
</script>

<template>
  <div class="row">
    <div class="col-12">
      <div
        class="page-title-box d-flex align-items-center justify-content-between"
      >
        <h2
          :id="id"
          :ref="id"
          :class="`mb-0 mx-auto mx-sm-0 font-weight-bold text-${variant}`"
          >{{ value }}</h2
        >
      </div>
    </div>
  </div>
</template>
