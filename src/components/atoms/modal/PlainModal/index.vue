<script>
import { BModal } from 'bootstrap-vue'
export default {
  name: 'PlainModal',
  props: {
    id: { default: 'plain-modal' },
    size: { type: String, default: 'md' },
    is_centered: { type: Boolean, default: true },
    is_static: { default: false },
    is_lazy: { default: false },
    value: { type: Boolean, default: false },
    has_close_button: { default: false },
    close_button_position: { default: 'right' },
    close_button_color: { default: 'text-danger' },
    close_button_icon: { default: 'bx bxs-x-circle' },
    close_button_text: { default: 'Tutup pop up' },
    close_button_size: { default: '24' },
    additional_class_content: { default: '' },
    additional_class_dialog: { default: '' },
  },
  components: {
    BModal,
  },
  methods: {
    toggleCloseModal() {
      this.$emit('close')
    },
  },
  computed: {
    local_value: {
      get: function () {
        return this.value
      },
      set: function (value) {
        return this.$emit('input', value)
      },
    },
  },
}
</script>
<template>
  <b-modal
    :id="`${id}`"
    :ref="`${id}`"
    v-model="local_value"
    :size="size"
    :centered="is_centered"
    hide-header
    hide-footer
    :lazy="is_lazy"
    :static="is_static"
    @hidden="toggleCloseModal()"
    :dialog-class="`custom-dialog-mobile ${additional_class_dialog}`"
    :content-class="additional_class_content"
  >
    <div
      v-if="has_close_button"
      :class="`float-${close_button_position}`"
      :id="`${id}__close-wrapper`"
      :ref="`${id}__close-wrapper`"
    >
      <i
        v-b-tooltip
        :id="`${id}__close-button`"
        :ref="`${id}__close-button`"
        :title="close_button_text"
        @click="toggleCloseModal()"
        :class="`${close_button_icon} cursor-pointer ${close_button_color} font-size-${close_button_size}`"
      ></i>
    </div>
    <slot />
  </b-modal>
</template>

<style type="text/css">
@media screen and (max-width: 976px) {
  .custom-dialog-mobile {
    width: 95% !important;
    max-width: 95% !important;
    min-width: 95% !important;
  }
}
</style>
