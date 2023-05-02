<script>
export default {
  components: {
    ActiveButton: () => import('@/components/atoms/button/ActiveButton'),
  },
  props: {
    id: { default: '' },
    prepend_variant: { default: 'default' },
    prepend_type: { default: '' },
    prepend_text: { default: 'Filter berdasarkan' },
    prepend_text_color: { default: 'text-muted' },
    prepend_text_size: { default: '14' },
    prepend_icon: { default: '' },
    prepend_icon_color: { default: '' },
    prepend_icon_size: { default: '' },
    prepend_size: { default: 'md' },
    prepend_align: { default: 'ltr' },
    reset_variant: { default: 'danger' },
    reset_type: { default: 'outline' },
    reset_text: { default: 'Hapus Filter' },
    reset_text_color: { default: 'text-danger' },
    reset_text_size: { default: '14' },
    reset_align: { default: 'ltr' },
    reset_icon: { default: '' },
    reset_icon_size: { default: '' },
    reset_icon_color: { default: '' },
    has_shadow: { default: false },
    has_prepend_text: { default: false },
    has_reset_button: { default: false },
    is_vertical: { default: false },
    additional_class: { default: '' },
    additional_class_prepend: { default: '' },
    additional_class_reset: { default: '' },
  },
  methods: {
    passClearToParent() {
      this.$emit('clear', true)
    },
  },
}
</script>
<template>
  <b-button-group :id="id" :ref="id" :vertical="is_vertical">
    <active-button
      v-if="has_prepend_text"
      :id="`prepend-btn-group-${id}`"
      :ref="`prepend-btn-group-${id}`"
      :additional_class="{
        [additional_class_prepend]: true,
        'rounded-left': has_reset_button,
        'cursor-default border border-right-0': true,
      }"
      :text="prepend_text"
      :text_color="prepend_text_color"
      :text_size="prepend_text_size"
      :variant="prepend_variant"
      :icon="prepend_icon"
      :icon_color="prepend_icon_color"
      :icon_size="prepend_icon_size"
      :type="prepend_type"
      :align="prepend_align"
    />
    <slot />
    <active-button
      v-if="has_reset_button"
      :id="`reset-btn-group-${id}`"
      :ref="`reset-btn-group-${id}`"
      :additional_class="{
        [additional_class_reset]: true,
        'border rounded-right': true,
      }"
      @click="passClearToParent()"
      :text="reset_text"
      :text_color="reset_text_color"
      :text_size="reset_text_size"
      :variant="reset_variant"
      :icon="reset_icon"
      :icon_color="reset_icon_color"
      :icon_size="reset_icon_size"
      :type="reset_type"
      :align="reset_align"
    />
  </b-button-group>
</template>
<style scoped>
@media (min-width: 700px) {
  .dr-breakout-btn {
    display: none;
  }
  .dr-breakout {
    display: inline;
    background: transparent;
    box-shadow: none;
    border: none;
    position: relative;
    margin: 0;
  }
  .dr-breakout li {
    display: inline;
  }
  .dr-breakout li a {
    display: inline-block;
    padding: 6px 12px;
    margin-bottom: 0;
    font-size: 14px;
    font-weight: 400;
    line-height: 1.42857143;
    text-align: center;
    white-space: nowrap;
    vertical-align: middle;
    -ms-touch-action: manipulation;
    touch-action: manipulation;
    cursor: pointer;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
    background-image: none;
    border-radius: 4px;
    color: #fff;
    background-color: #507281;
  }
}
</style>
