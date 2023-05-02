<script>
export default {
  name: 'DisabledButton',
  props: {
    id: { default: 'id' },
    variant: { type: String, default: 'primary' },
    type: { type: String, default: '' },
    text: { type: String, default: 'Loading' },
    size: { type: String, default: 'md' },
    align: { type: String, default: 'ltr' },
    is_capitalize: { default: false },
    additional_class: { type: String, default: '' },
  },
}
</script>

<template>
  <button
    :class="`${is_capitalize ? 'text-uppercase' : ''} shadows btn btn${
      type ? '-' + type : ''
    }-${variant} btn-${size} ${additional_class}`"
    disabled
    :id="id"
    :ref="id"
  >
    <span
      :id="`content-${id}`"
      class="d-flex align-items-center justify-content-center"
      :class="`${align === 'rtl' ? 'flex-row-reverse' : ''}`"
    >
      {{ text }}
      <div
        class="spinner-border ml-2"
        role="status"
        aria-hidden="true"
        style="height: 15px; width: 15px"
      ></div>
    </span>
  </button>
</template>
