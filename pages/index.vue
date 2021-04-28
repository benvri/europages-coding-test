<template>
  <div> 
    <VCard class="mb-4">
      <VSystemBar />
      <VBanner single-line>
        Filter the restaurants by ratings
        <template #actions>
          <VSelect
            :items="ratingFilterItems"
            v-model="minimumRating"
            v-on:change="selectEvent"
            filled
            label="choose your rating"
          />
        </template>
      </VBanner>
    </VCard>
    <div class="d-flex flex-wrap"> 
      <VaCompanyCard
        v-for="company of selectedCompanies"
        :key="company.id"
        :company="company"
      />
    </div>
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
      selectedCompanies : [],
      minimumRating: 0,
    }
  },
  async fetch() {
    try {
      const companies = await this.$axios.$get(`/companies`)
      this.companies = companies
      this.selectedCompanies = this.companies
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
  methods:{
    selectEvent(){
      this.selectedCompanies = []
      for(const company of this.companies){
        let mean = 0
        for(const review of company.reviews){
          mean += review.rating
        }
        company.meanRating = mean / company.reviews.length
        if(company.meanRating > this.minimumRating){
          this.selectedCompanies.push(company)
        }
      }
    }
  }
}
</script>

<style lang="scss" scoped>
@import '~vuetify/src/styles/styles.sass';
@media #{map-get($display-breakpoints, 'md-and-up')} {
  .va-company-card {
    width: 32%;
    margin:4px;
  }
}
@media #{map-get($display-breakpoints, 'sm-and-down')} {
  .va-company-card {
    width: 100%;
  }
}
</style>
