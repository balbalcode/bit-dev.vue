<script>
export default {
  name: 'TextInput',
  props: {
    id: { type: String, default: '' },
    is_disabled: { type: Boolean },
    format: { type: String, default: '' },
    size: { type: String, default: 'md' },
    value: { default: '' },
    placeholder: { type: String, default: '' },
    is_autocomplete: { type: Boolean },
    is_error: { type: Boolean },
    is_password: { type: Boolean },
    max_length: { default: 524288 },
    additional_class: { type: String, default: '' },
  },

  data: () => ({
    show_password: false,
  }),

  computed: {
    local_value: {
      get: function () {
        let value = `${this.value}`
        return this.formatText(value)
      },
      set: function (value) {
        let text = this.formatText(value)
        this.$emit('input', text ?? '')
        this.$forceUpdate()
      },
    },

    input_class() {
      let inputClass = `form-control-${this.size} ${this.additional_class}`
      if (this.is_error) inputClass += ' is-invalid'
      return inputClass
    },
  },

  methods: {
    submitToParent() {
      this.$emit('submit')
    },

    keyupToParent() {
      this.$emit('keyup')
    },

    formatText(text) {
      let value = text
      if (Array.isArray(this.format)) {
        this.format.forEach((item) => {
          value = this.reformatText(item, value)
        })
      } else {
        value = this.reformatText(this.format, value)
      }
      return value
    },

    reformatText(type, text) {
      switch (type) {
        case 'number':
          text = this.$utility.removeLetter(text)
          return this.$utility.convertToNumber(text)
        case 'currency':
          text = this.$utility.removeLetter(text)
          text = this.$utility.convertToNumber(text)
          return this.$utility.convertToRupiah(text)
        case 'license_plate':
          text = this.$utility.removeSpace(text)
          text = this.$utility.removeSymbols(text)
          return this.$utility.setUpperCaseLetter(text)
        case 'letter':
          return this.$utility.removeNumber(text)
        case 'no-symbol':
          return this.$utility.removeSymbols(text)
        case 'no-letter':
          return this.$utility.removeLetter(text)
        case 'no-space':
          return this.$utility.removeSpace(text)
        default:
          return text
      }
    },

    toggleShowPassword() {
      this.show_password = !this.show_password
    },
  },
}
</script>
<template>
  <div
    :id="`wrapper-${id}`"
    :class="{ 'input-group input-group-merge': is_password }"
  >
    <input
      v-model="local_value"
      @keyup.enter="submitToParent()"
      class="form-control"
      :class="input_class"
      :id="id"
      :name="id"
      :disabled="is_disabled"
      :maxlength="max_length"
      :placeholder="placeholder"
      @keyup="keyupToParent()"
      :autocomplete="is_autocomplete ? 'on' : 'off'"
      :type="is_password && !show_password ? 'password' : 'text'"
    />
    <div
      v-if="is_password"
      :id="`toggle-password-${id}`"
      class="input-group-append"
      data-password="false"
    >
      <div
        :id="`action-toggle-password-${id}`"
        class="input-group-text cursor-pointer"
        @click="toggleShowPassword()"
      >
        <i
          :id="`icon-password-${id}`"
          class="bx"
          :class="{ 'bx-hide': show_password, 'bx-show': !show_password }"
        ></i>
      </div>
    </div>
  </div>
</template>
<style>
input::placeholder {
  color: #cecece !important;
}
</style>
