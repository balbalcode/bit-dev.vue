<script>
export default {
  name: 'DateInput',
  components: {
    DatePicker: () => import('vue2-datepicker'),
  },
  props: {
    id: { type: String, default: '' },
    placeholder: { type: String },
    format: { type: String },
    value: { default: () => new Date() },
    value_type: { type: String, default: 'date' },
    type: { type: String, default: 'date' },
    is_disabled: { type: Boolean },
    is_range: { type: Boolean },
    disabled_date: { type: String, default: '' },
    range_separator: { type: String, default: ' hingga ' },
    manual_opener: { type: Boolean },
    opener_state: { type: Boolean },
    additional_class: { type: String, default: '' },
  },
  computed: {
    open_state: {
      get: function () {
        return this.opener_state
      },
      set: function (value) {
        this.$emit('open', value)
      },
    },
    local_value: {
      get: function () {
        return this.value
      },
      set: function (value) {
        this.$emit('input', value)
      },
    },
  },
  methods: {
    generateDisabledDate(date) {
      let today = new Date()
      today.setHours(0, 0, 0)
      const tomorrow = new Date(
        today.getFullYear(),
        today.getMonth(),
        today.getDate() + 1
      )
      const this_week = new Date(
        today.getFullYear(),
        today.getMonth(),
        today.getDate() - today.getDay()
      )
      const next_week = new Date(
        today.getFullYear(),
        today.getMonth(),
        today.getDate() - today.getDay() + 7
      )
      const this_month = new Date(today.getFullYear(), today.getMonth(), 1)
      const next_month = new Date(today.getFullYear(), today.getMonth() + 1, 1)

      switch (this.disabled_date) {
        case 'today':
          return date >= today && date < tomorrow
        case 'before-today':
          return date < today
        case 'after-today':
          return date >= tomorrow
        case 'this-week':
          return date >= this_week && date < next_week
        case 'before-this-week':
          return date < this_week
        case 'after-this-week':
          return date >= next_week
        case 'not-this-week':
          return date < this_week || date >= next_week
        case 'this-month':
          return date >= this_month && date < next_month
        case 'before-this-month':
          return date < this_month
        case 'after-this-month':
          return date >= next_month
        case 'not-this-month':
          return date < this_month || date >= next_month
        case 'this-year':
          return date.getFullYear() == today.getFullYear()
        case 'before-this-year':
          return date.getFullYear() < today.getFullYear()
        case 'after-this-year':
          return date.getFullYear() > today.getFullYear()
        case 'not-this-year':
          return (
            date.getFullYear() < today.getFullYear() ||
            date.getFullYear() > today.getFullYear()
          )
        default:
          return false
      }
    },
  },
}
</script>
<template>
  <date-picker
    v-if="manual_opener"
    :format="format"
    v-model="local_value"
    :type="type"
    :placeholder="placeholder"
    :class="additional_class"
    :disabled-date="generateDisabledDate"
    :default-value="value"
    :value-type="value_type"
    :clearable="false"
    :range-separator="range_separator"
    :editable="false"
    :disabled="is_disabled"
    :range="is_range"
    :id="id"
    :ref="`${id}-manual-open`"
    :open.sync="open_state"
  >
    <template slot="icon-calendar">
      <i
        :id="`${id}-icon`"
        class="bx"
        :class="{ 'bx-time': type === 'time', 'bx-calendar': type !== 'time' }"
      ></i>
    </template>
  </date-picker>
  <date-picker
    v-else
    :format="format"
    v-model="local_value"
    :type="type"
    :placeholder="placeholder"
    :class="additional_class"
    :disabled-date="generateDisabledDate"
    :default-value="value"
    :value-type="value_type"
    :clearable="false"
    :range-separator="range_separator"
    :editable="false"
    :disabled="is_disabled"
    :range="is_range"
    :id="id"
    :ref="`${id}-normal`"
  >
    <template slot="icon-calendar">
      <i
        :id="`${id}-icon`"
        class="bx"
        :class="{ 'bx-time': type === 'time', 'bx-calendar': type !== 'time' }"
      ></i>
    </template>
  </date-picker>
</template>
<style>
.disabled > .mx-input-wrapper > input {
  background: #f4f4f4 !important;
  color: #74788d !important;
}
</style>
