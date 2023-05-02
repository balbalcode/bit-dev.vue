<template>
  <div :class="`bg-light rounded ${additional_class}`">
    <h5
      id="accordion-header"
      class="font-weight-bolder rounded-top px-2 py-2 bg-primary-20 cursor-pointer my-3"
      @click="is_collapse = !is_collapse"
      :class="{ rounded: !is_collapse }"
    >
      {{ title }}
      <span class="float-right">
        <i
          id="accordion-header-icon"
          :class="`bx bx-chevron-${is_collapse ? 'up' : 'down'} bx-sm`"
        />
      </span>
    </h5>
    <div
      v-if="is_collapse"
      id="accordion-content"
      :class="additional_class_content"
    >
      <slot />
    </div>
  </div>
</template>

<script>
export default {
  props: {
    title: { default: '' },
    collapsed: { default: true }, // collapse initial condition
    additional_class: { default: '' },
    additional_class_content: { default: '' },
  },
  data() {
    return {
      is_collapse: this.collapsed,
    }
  },
}
</script>
