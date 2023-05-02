<script>
export default {
  components: {
    ActiveButton: () => import('@/components/atoms/button/ActiveButton'),
  },
  props: {
    id: { default: 'content-span' },
    image: { type: String, default: 'motorcycle.png' },
    size: { type: String, default: 'md' },
    has_animation: { type: Boolean, default: true },
    caption: { type: String, default: 'Loading ...' },
    margin_span: { type: String, default: '10vh 0' },
    additional_class: { type: String, default: '' },
    has_reload_button: { type: Boolean, default: false },
  },
  data: () => ({
    image_size: '96px',
    height: {
      sm: '64px',
      md: '96px',
      lg: '128px',
      xl: '172px',
    },
  }),
  methods: {
    passReloadToParent() {
      this.$emit('reload')
    },
  },
}
</script>
<template>
  <div
    :id="`wrapper-${id}`"
    class="col-12 d-flex flex-column align-items-center"
    :class="{ [additional_class]: true }"
    :style="{ margin: margin_span }"
  >
    <img
      :id="`img-${id}`"
      :src="`${$utility.getAssetUrl()}/${image}`"
      :height="height[size]"
      :class="{ bouncing: has_animation }"
    />
    <h5
      :id="`caption-${id}`"
      class="font-weight-bold text-black-50 mx-auto mt-2 text-center"
      >{{ caption }}</h5
    >
    <active-button
      v-if="has_reload_button"
      :id="`reload-btn-${id}`"
      :ref="`reload-btn-${id}`"
      text="Muat Ulang"
      text_color="text-light"
      size="sm"
      additional_class="mt-2 py-1 px-3"
      @click="passReloadToParent"
    />
  </div>
</template>
