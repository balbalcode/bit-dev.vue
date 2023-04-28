<script>
import appConfig from '@src/app.config'

export default {
  page: {
    // All subcomponent titles will be injected into this template.
    titleTemplate(title) {
      title = typeof title === 'function' ? title(this.$store) : title
      return title ? `${title} | ${appConfig.title}` : appConfig.title
    },
  },
  components: {},
}
</script>

<template>
  <div id="app"></div>
</template>

<!-- This should generally be the only global CSS in the app. -->
<style lang="scss">
@import '~normalize.css/normalize.css';
@import '@design';
</style>
