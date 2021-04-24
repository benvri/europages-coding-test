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
    <v-rating
      empty-icon="mdi-heart-outline"
      full-icon="mdi-heart"
      half-icon="mdi-heart-half"
      readonly
      color="red"
      background-color="red"
      length="5"
      size="35"
      :value=meanRating
    ></v-rating>
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
    meanRating: function () {
      let mean = 0
      for(const review of this.company.reviews){
        mean += review.rating
      }
      return mean / this.company.reviews.length
    }
  },
}
</script>
