<template>
  <div>
    <VCard class="mb-4">
      <VSystemBar />
      <VAlert text type="success">
        TODO: BONUS - this is a bonus! (do that at the very end if you're
        willing to)<br />
        Implement the rating filter <br />
        Remove the <code>VAlert</code> when it's done
      </VAlert>
      <VBanner single-line>
        Filter the restaurants by ratings
        <template #actions>
          <VSelect
            :items="ratingFilterItems"
            filled
            label="choose your rating"
          />
        </template>
      </VBanner>
    </VCard>
    <VAlert text type="error">
      TODO: MAJOR – this should be presentend as a grid<br />
      • 3 columns max<br />
      • 1 column on small devices <br />
      • DO NOT USE
      <a
        href="https://vuetifyjs.com/en/components/grids/"
        target="_blank"
        rel="noopener noreferrer"
      >
        Vuetify's grid components
      </a>
      <br />
      Remove the <code>VAlert</code> when it's done
    </VAlert>
    <VaCompanyCard
      v-for="company of companies"
      :key="company.id"
      :company="company"
    />
  </div>
</template>

<script>
import VaCompanyCard from '~/components/company-card.vue'

export default {
  name: `page-home`,
  components: {
    VaCompanyCard,
  },
  data() {
    return {
      companies: [],
      minimumRating: 0,
    }
  },
  async fetch() {
    try {
      const companies = await this.$axios.$get(`/companies`)
      this.companies = companies
    } catch (error) {
      this.$nuxt.error(error)
    }
  },
  computed: {
    ratingFilterItems() {
      return [
        { text: `all restaurants`, value: 0 },
        { text: `5 stars`, value: 5 },
        { text: `4 stars or more`, value: 4 },
        { text: `3 stars or more`, value: 3 },
        { text: `2 stars or more`, value: 2 },
        { text: `1 star or more`, value: 1 },
      ]
    },
  },
}
</script>

<style lang="scss" scoped>
@import '~vuetify/src/styles/styles.sass';

// TODO: MAJOR – this should be presentend as a grid
// 3 rows max
// 1 row on small devices

// NOTE: you  can use Vuetify breakpoints to handle the ≠ layouts
// • https://vuetifyjs.com/en/features/breakpoints/
// @media #{map-get($display-breakpoints, 'lg-and-up')} {
//   do stuff…
// }
</style>
