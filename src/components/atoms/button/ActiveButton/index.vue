<script>
export default {
  name: 'ActiveButton',
  props: {
    id: { default: '' },
    variant: { default: 'primary' },
    type: { default: '' },
    text: { default: 'Simpan' },
    text_color: { default: 'text-dark' },
    size: { default: 'md' },
    icon: { default: '' },
    icon_size: { default: '' },
    icon_color: { default: '' },
    align: { default: 'ltr' },
    has_shadow: { default: false },
    is_disabled: { default: false },
    is_capitalize: { default: false },
    additional_class: { default: '' },
  },
  methods: {
    passToParent() {
      this.$emit('click')
    },
    formatButtonClass() {
      let button_classes = `btn${this.type ? '-' + this.type : ''}-${
        this.variant
      } btn-${this.size} ${this.text_color}`

      let conditional_classes = {
        'text-uppercase': this.is_capitalize,
        shadow: this.has_shadow,
        [button_classes]: true,
      }

      if (typeof this.additional_class === 'string')
        conditional_classes[this.additional_class] = true
      else if (typeof this.additional_class === 'object')
        Object.assign(conditional_classes, this.additional_class)

      return conditional_classes
    },
  },
}
</script>

<template>
  <button
    @click="passToParent"
    :id="id"
    class="btn"
    :class="formatButtonClass()"
    :disabled="is_disabled"
  >
    <span
      :id="`content-${id}`"
      class="d-flex align-items-center justify-content-center"
      :class="{ 'flex-row-reverse': align === 'rtl' }"
    >
      <span class="flex-fill">{{ text }}</span>
      <i
        :id="`icon-${id}`"
        v-if="icon"
        :class="`${icon_color} ${icon} font-size-${icon_size} mx-1`"
      ></i>
    </span>
  </button>
</template>
