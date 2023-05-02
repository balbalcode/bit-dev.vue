<script>
export default {
  name: 'ExportButton',
  components: {
    JsonExcel: () => import('vue-json-excel'),
  },
  props: {
    variant: { default: 'primary' },
    type: { default: '' },
    text: { default: 'Download Laporan' },
    text_color: { default: 'text-dark' },
    size: { default: 'md' },
    icon: { default: '' },
    icon_size: { default: '' },
    icon_color: { default: '' },
    id: { default: '' },
    align: { default: 'ltr' },
    has_shadow: { default: false },
    is_disabled: { default: false },
    has_default_padding: { default: false },
    additional_class: { default: '' },
    fields: { default: () => ({}) },
    is_capitalize: { default: true },
    content: { default: () => [] },
    export_name: { default: '' },
    export_format: { default: 'xls' },
  },
  data: () => ({
    period: new Date(),
  }),
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
    :id="`btn-${id}`"
    :disabled="is_disabled"
    class="btn"
    :class="formatButtonClass()"
  >
    <json-excel
      :fields="fields"
      :data="content"
      :name="`${export_name}.${export_format}`"
      :id="id"
      :ref="id"
    >
      <span
        :id="`content-${id}`"
        class="d-flex align-items-center justify-content-center"
        :class="{ 'flex-row-reverse': align === 'rtl' }"
      >
        <span>{{ text }}</span>
        <i
          :id="`icon-${id}`"
          v-if="icon"
          :class="`${icon_color} ${icon} font-size-${icon_size} mx-1`"
        ></i>
      </span>
    </json-excel>
  </button>
</template>
