<script>
import { BFormTags } from 'bootstrap-vue'
export default {
  components: { BFormTags },
  props: {
    id: { type: String, default: '' },
    is_disabled: { type: Boolean },
    size: { type: String, default: 'md' },
    variant: { type: String, default: 'primary' },
    value: { default: () => [] },
    placeholder: { type: String, default: 'Masukkan data' },
    tag_separator: { type: String, default: ' ;,' },
    is_error: { type: Boolean },
    is_removable_tags: { type: Boolean, default: true },
    non_removable_tags: { default: () => [] },
    limit: { type: [String, Number], default: 100 },
    additional_class: { type: String, default: '' },
    additional_class_tag: { type: String, default: '' },
  },
  computed: {
    local_value: {
      get: function () {
        return this.value
      },
      set: function (value) {
        if (this.processCheckDeleted()) this.$emit('input', value)
      },
    },
    input_class() {
      let inputClass = this.additional_class
      if (this.is_error) inputClass += ' is-invalid'
      return inputClass
    },
  },

  methods: {
    submitToParent() {
      this.$emit('submit')
    },

    processCheckDeleted(value) {
      // return true if new input still part of non removable data
      return this.non_removable_tags.every((item) => value.includes(item))
    },
  },
}
</script>
<template>
  <b-form-tags
    :input-id="id"
    :input-class="input_class"
    :disabled="is_disabled"
    :placeholder="placeholder"
    :size="size"
    no-add-on-enter
    :no-tag-remove="!is_removable_tags"
    :ref="id"
    v-model="local_value"
    :remove-on-delete="is_removable_tags"
    :separator="tag_separator"
    :limit="limit"
    :tag-variant="variant"
    duplicate-tag-text="Data yang anda masukkan tidak boleh sama, Atur kembali pada: "
    add-on-change
    :limit-tags-text="`Maksimum data yang dimasukkan adalah ${limit} data`"
    :tag-class="`font-weight-bold ${additional_class_tag}`"
    @keyup.enter="submitToParent()"
  >
  </b-form-tags>
</template>
<style>
input::placeholder {
  color: #cecece !important;
}
</style>
