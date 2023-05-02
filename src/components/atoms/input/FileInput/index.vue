<script>
import { s3Methods } from '~/store/helperActions'
import CropperImage from '@/components/atoms/image/CropperImage'
import { BModal } from 'bootstrap-vue'

export default {
  name: 'FileInput',
  components: {
    BModal,
    ActiveButton: () => import('~/components/atoms/button/ActiveButton'),
    DisabledButton: () => import('~/components/atoms/button/DisabledButton'),
    PlainLabel: () => import('~/components/atoms/label/PlainLabel'),
    CropperImage,
  },
  props: {
    id: { default: '' },
    value: { default: '' },
    module: { default: 'mgmt/uploaded' },
    files: { default: () => [] },
    file_prefix: { default: '' },
    max_files: { default: 1 },
    max_size: { default: 3000000 },
    file_type: { default: '.png,.jpeg,.jpg' },
    additional_class: { default: '' },
    button_text: { default: 'Unggah berkas' },
    button_variant: { default: 'primary' },
    button_type: { default: '' },
    button_icon: { default: 'bx bx-cloud-upload' },
    button_size: { default: 'md' },
    button_class: { default: '' },
    loading_text: { default: 'Sedang mengunggah' },
    has_cropper: { type: Boolean },
    has_save_button: { type: Boolean },
    is_disabled: { type: Boolean },
    is_auto_upload: { default: true },
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
  data: () => ({
    helper: {
      errors: [],
      cropper_image: false,
      is_saved: false,
      loading_upload: false,
    },
    modal: {
      modal_cropper: false,
    },
    cropper_image: '',
  }),
  methods: {
    ...s3Methods,

    async setFiles(e) {
      if (e) {
        let files
        if (e.target && e.target.files) files = e.target.files
        else if (e.dataTransfer && e.dataTransfer.files)
          files = e.dataTransfer.files

        if (files && files.length) {
          await this.processReadFile(files)
        }
      }
    },

    toggleLoadingUpload() {
      this.helper.loading_upload = !this.helper.loading_upload
    },

    toggleModalCropper() {
      this.modal.modal_cropper = !this.modal.modal_cropper
      setTimeout(() => {
        this.helper.cropper_image = !this.helper.cropper_image
      }, 1500)
    },

    processValidateSize(file) {
      let size = file.size
      if (size > this.max_size) {
        const max = Math.round(this.max_size / 1000)
        this.helper.errors.push(
          `Ukuran berkas ${file.name} harus lebih kecil dari ${max}KB`
        )
      }
    },

    processValidateFormat(file) {
      let type = file.name.split('.')
      type = `.${type[type.length - 1].toLowerCase()}`
      if (!this.file_type.split(',').includes(type)) {
        this.helper.errors.push(`Format berkas ${type} tidak diizinkan`)
      }
    },

    processValidateFile(files) {
      this.helper.errors = []
      if (files.length <= this.max_files) {
        for (let index = 0; index < files.length; index++) {
          this.processValidateFormat(files[index])
          this.processValidateSize(files[index])
        }
      } else {
        this.helper.errors.push(
          `Maksimal berkas yang diunggah adalah ${this.max_files}`
        )
      }
    },

    async processReadFile(files) {
      let reader = new FileReader()
      this.helper.is_saved
      this.processValidateFile(files)
      if (this.helper.errors.length < 1) {
        this.helper.is_saved = false
        this.$emit('files', files)
        if (this.has_cropper && this.max_files == 1) {
          reader.onload = (event) => {
            this.cropper_image = event.target.result
          }
          reader.readAsDataURL(files[0])
          this.toggleModalCropper()
        } else {
          for (let index = 0; index < files.length; index++) {
            reader.readAsDataURL(files[index])
            if (this.is_auto_upload) await this.processUploadFile(files[index])
          }
        }
      }
    },

    //
    async processUploadAllFile() {
      for (let index = 0; index < this.files.length; index++) {
        await this.processUploadFile(this.files[index])
      }
    },

    async processUploadFile(file) {
      try {
        this.toggleLoadingUpload()
        if (typeof file === 'string')
          file = this.$utility.convertDataURItoBlob(file)

        let type = file.name ? file.name.split('.') : file.type.split('/')
        let full_file = this.$utility.generateFilePathPreSigned(
          this.module,
          type[type.length - 1],
          this.file_prefix
        )
        let pre_signed = await this.getPreSignedPostUrl({
          object_name: full_file,
        })
        pre_signed = pre_signed.values
        await this.processS3Upload(pre_signed, file)
        this.local_value = pre_signed.fields.key
        this.toggleLoadingUpload()
      } catch (error) {
        this.$utility.setErrorContextSentry(error)
        this.$sentry.captureMessage(
          `${error.message} at processUploadFile in FileInput`
        )
      }
    },

    async processS3Upload(pre_signed, file) {
      try {
        await this.uploadDataToS3({ preSigned: pre_signed, file: file })
        this.helper.is_saved = true
      } catch (error) {
        this.$utility.setErrorContextSentry(error)
        this.$sentry.captureMessage(
          `${error.message} at processS3Upload in FileInput`
        )
      }
    },
  },
}
</script>
<template>
  <div :class="additional_class">
    <input
      :id="id"
      type="file"
      ref="file"
      style="display: none"
      @change="setFiles($event)"
      :accept="file_type"
      :multiple="max_files > 1"
    />
    <disabled-button
      v-if="helper.loading_upload"
      :id="`btn_loading_upload_${id}`"
      :ref="`btn_loading_upload_${id}`"
      :size="button_size"
      :additional_class="button_class"
      :text="loading_text"
      :variant="button_variant"
    />
    <template v-else>
      <active-button
        v-if="!files.length"
        :id="`btn_upload_${id}`"
        :ref="`btn_upload_${id}`"
        :additional_class="button_class"
        :variant="button_variant"
        :icon="button_icon"
        align="rtl"
        icon_size="16"
        :type="button_type"
        :text="button_text"
        @click="$refs.file.click()"
        :size="button_size"
        :is_disabled="is_disabled"
      />
      <active-button
        v-if="!is_auto_upload && files.length"
        :id="`btn_edit_${id}`"
        :additional_class="button_class"
        :variant="button_variant"
        icon="bx bx-pencil"
        align="rtl"
        icon_size="16"
        type="outline"
        text="Edit berkas"
        @click="$refs.file.click()"
        :size="button_size"
      />
      <active-button
        v-if="
          !is_auto_upload &&
          has_save_button &&
          files.length &&
          !helper.errors.length &&
          !helper.is_saved
        "
        :additional_class="button_class"
        :variant="button_variant"
        icon="bx bx-save"
        align="rtl"
        icon_size="16"
        :text="button_text"
        @click="processUploadAllFile"
        :size="button_size"
        :id="`btn_save_${id}`"
      />
    </template>
    <small class="text-danger" v-if="helper.errors.length">
      <li
        v-for="(error, index) in helper.errors"
        :key="`error-${index}`"
        class="ml-3 my-1"
      >
        {{ error }}
      </li>
    </small>

    <b-modal
      v-if="has_cropper"
      v-model="modal.modal_cropper"
      hide-footer
      hide-header
      size="md"
      centered
    >
      <div v-if="helper.cropper_image">
        <cropper-image
          :image="cropper_image"
          :is_auto_upload="is_auto_upload"
          :id="`crop-${id}`"
          @upload="processUploadFile"
          @close="toggleModalCropper"
        />
      </div>
      <div v-else class="w-100 text-center py-5 my-5">
        <b-spinner variant="primary" />
      </div>
    </b-modal>
  </div>
</template>
