<script>
export default {
  name: 'SelectInput',
  components: {
    Multiselect: () => import('vue-multiselect'),
  },
  props: {
    id: { type: String },
    value: { default: () => ({}) },
    placeholder: { default: 'Pilih Data' },
    options: { type: Array, default: () => [] },
    search_by: { default: 'text' },
    is_searchable: { type: Boolean },
    is_multiple: { type: Boolean },
    is_nullable: { type: Boolean },
    is_disabled: { type: Boolean },
    is_preserve_search: { type: Boolean },
    is_close_on_select: { default: true },
    is_force_open: { type: Boolean },
    option_position: { default: '' },
    has_custom_caption: { type: Boolean },
    custom_caption: { default: 'Opsi dipilih' },
    additional_class: { default: '' },
  },
  computed: {
    local_value: {
      get: function () {
        return this.value
      },
      set: function (value) {
        this.$emit('input', value)
      },
    },
  },
  mounted() {
    if (this.is_force_open) {
      setTimeout(() => {
        this.$refs[this.id].activate()
      }, 500)
    }
  },
  methods: {
    passOpenToParent() {
      this.$emit('open', true)
    },
    passCloseToParent() {
      if (this.is_force_open) {
        this.$refs[this.id].activate()
      } else {
        this.$emit('close', true)
      }
    },
  },
}
</script>
<template>
  <multiselect
    v-model="local_value"
    :options="options"
    :disabled="is_disabled"
    :ref="id"
    :track-by="search_by"
    :searchable="is_searchable"
    select-label="Pilih data"
    :placeholder="placeholder"
    :allow-empty="is_nullable"
    :label="search_by"
    :id="id"
    @open="passOpenToParent()"
    :multiple="is_multiple"
    deselect-label="Hapus pilihan"
    selectedLabel="Dipilih"
    @close="passCloseToParent()"
    :open-direction="option_position"
    :close-on-select="is_close_on_select"
    :class="additional_class"
  >
    <template #caret v-if="is_searchable">
      <span id="handler-caret-searchable" class="d-none"></span>
    </template>
    <template #tag v-if="is_multiple">
      <span id="handler-tag-multiple" class="d-none"></span>
    </template>
    <!-- custom caption for multiple selection  -->
    <template v-if="has_custom_caption" #selection="{ values }">
      <span id="handler-selection" class="multiselect__single">
        {{ values.length }} {{ custom_caption }}
      </span>
    </template>
    <template slot="noOptions">List tidak tersedia</template>
    <template slot="noResult">Data tidak ditemukan</template>
  </multiselect>
</template>
<style lang="scss">
.multiselect {
  min-height: auto;
}

.multiselect.multiselect--disabled {
  background: #fff !important;

  > .multiselect__tags {
    background: #ededed !important;

    > .multiselect__single {
      background: #ededed !important;
    }
  }

  > .multiselect__select {
    height: 28px !important;
    top: 1px !important;
  }
}

.multiselect__select::before {
  z-index: 2 !important;
  top: 55% !important;
}

.multiselect__single {
  white-space: nowrap;
  overflow: hidden;
  padding-right: 1em;
  text-overflow: ellipsis;
}

div.multiselect__content-wrapper {
  z-index: 9999 !important;
}

.multiselect__tag-icon:focus,
.multiselect__tag-icon:hover {
  background: var(--danger);
}
</style>
