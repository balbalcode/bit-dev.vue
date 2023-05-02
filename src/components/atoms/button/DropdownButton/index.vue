<script>
import { BDropdown } from 'bootstrap-vue'
export default {
  components: {
    'b-dropdown': BDropdown,
  },
  props: {
    id: { default: '' },
    options: { default: () => [] },
    text: { type: String, default: '' },
    variant: { type: String, default: 'primary' },
    type: { type: String, default: '' },
    size: { type: String, default: 'md' },
    position: { type: String, default: 'left' },
    additional_class: { type: String, default: 'text-dark' },
    is_capitalize: { default: true },
    is_disabled: { default: false },
    has_custom_action: { default: false },
    caret: { type: String, default: 'bx bx-caret-down' },
  },
  data: () => ({
    current_text: '',
    is_right: false,
    is_top: false,
  }),
  mounted() {
    this.toggleDropType()
    this.current_text = this.text
  },
  methods: {
    passFunctionToParent(value) {
      this.current_text = value.text
      this.$emit('update', value.value)
    },

    toggleDropType() {
      if (this.position === 'right') {
        this.is_right = true
        this.is_top = false
      } else if (this.position === 'top') {
        this.is_right = false
        this.is_top = true
      }
    },
  },
}
</script>

<template>
  <b-dropdown
    :id="id"
    :ref="id"
    :right="is_right"
    :dropup="is_top"
    :variant="`${variant}${type ? '-' + type : ''}`"
    :menu-class="`${is_capitalize ? 'text-uppercase' : ''} ${additional_class}`"
    :disabled="is_disabled"
    :size="size"
  >
    <template #button-content>
      {{ current_text }} <i :class="`${caret}`"></i>
    </template>
    <template v-if="has_custom_action">
      <slot />
    </template>
    <b-dropdown-item
      v-else
      v-for="(option, index) in options"
      :key="index"
      :ref="`${id}-item-${index}`"
      :id="`${id}-item-${index}`"
      @click="passFunctionToParent(option)"
    >
      <i
        v-if="option.icon"
        :id="`${id}-item-${index}-icon`"
        :class="`${option.icon}`"
      ></i>
      {{ option.text }}
    </b-dropdown-item>
  </b-dropdown>
</template>
<style>
.dropdown-menu {
  border: 1px solid rgba(0, 0, 0, 0.15);
  box-shadow: 0 1.5px 3px 0 rgba(0, 0, 0, 0.16);
}

.dropdown-item:hover {
  background: var(--primary);
}

.btn:focus {
  box-shadow: none !important;
}
</style>
