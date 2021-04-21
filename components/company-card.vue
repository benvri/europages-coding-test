<template>
  <VCard class="va-company-card d-flex flex-column">
    <VImg
      height="100"
      :src="company.photos[0]"
      class="flex-shrink-0 flex-grow-0"
    />
    <VCardTitle class="headline">
      {{ company.name }}
    </VCardTitle>
    <VAlert text type="warning" class="mx-4 d-inline-block align-self-start">
      TODO: MINOR – display the average rating of the reviews
      <br />
      <a
        href="https://vuetifyjs.com/en/components/ratings/"
        target="_blank"
        rel="noopener noreferrer"
      >
        Use the Vuetify Rating component
      </a>
      <br />
      Remove the <code>VAlert</code> when it's done
    </VAlert>
    <VCardText>
      <VaComponayLocation :location="company.location" />
    </VCardText>
    <VCardActions class="mt-auto">
      <VBtn
        color="primary"
        nuxt
        :to="`/companies/${company.id}`"
        block
        outlined
      >
        see more
      </VBtn>
    </VCardActions>
  </VCard>
</template>

<script>
import VaComponayLocation from '~/components/company-location.vue'

export default {
  name: `va-company-card`,
  components: { VaComponayLocation },
  props: {
    company: { type: Object, default: () => ({}) },
  },
  computed: {
    // TODO: we should be able to have the average of all ratings
  },
}
</script>
