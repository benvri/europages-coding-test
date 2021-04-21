<template>
  <VApp>
    <h1 class="text-h3">
      {{ errorMessage }}
    </h1>
    <NuxtLink to="/"> Home page </NuxtLink>
    <VAlert text type="error" class="mt-6">
      TODO: MAJOR – when clicking on “see more” we see an error layout<br />
      This shouldn't be the case
      <br />
      Remove the <code>VAlert</code> when it's done
    </VAlert>
  </VApp>
</template>

<script>
export default {
  layout: 'page-error',
  props: {
    error: {
      type: Object,
      default: null,
    },
  },
  data() {
    return {
      pageNotFound: '',
      otherError: '',
    }
  },
  head() {
    const title = this.errorMessage
    return {
      title,
    }
  },
  computed: {
    errorMessage() {
      return this.error.statusCode === 404
        ? `404 Not Found`
        : `An error occurred`
    },
  },
}
</script>
