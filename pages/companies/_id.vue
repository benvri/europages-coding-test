<template>
  <v-container class="grey lighten-5">
    <v-row>
      <v-col cols="12" sm="12" md="8">
        <VSkeletonLoader v-if="$fetchState.pending" type="card" />
        <VCard v-else>
          <VImg
            v-for="photo in company.photos"
            :key="photo"
            :src="company.photos[0]"
            height="250"
            class="white--text"
            gradient="to top, rgba(0,0,0,.1), rgba(0,0,0,.5)"
          >
            <VCardTitle class="text-h3">{{ company.name }}</VCardTitle>
          </VImg>
          <VCardText>
            <div class="page-company__contact-info">
              <VaCompanyLocation :location="company.location" />
              <div>
                <PhPhone size="24" />
                <p class="text-body-1">
                  <a :href="'tel:'+ company.phone">{{ company.phone | phone }}</a>
                </p>
              </div>
            </div>
          </VCardText>
        </VCard>
      </v-col>
      <v-col cols="12" sm="12" md="4">
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
        <ul class="pa-0">
          <VSkeletonLoader
            v-if="$fetchState.pending"
            type="article"
            elevation="2"
          />
          <VaCompanyReview v-else />
        </ul>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { PhPhone } from 'phosphor-vue'

import VaCompanyLocation from '~/components/company-location.vue'
import VaCompanyReview from '~/components/company-review.vue'

export default {
  name: `page-company`,
  components: { VaCompanyLocation, PhPhone, VaCompanyReview },
  data() {
    return {
      company: {},
      currentSlide: 0,
    }
  },
  async fetch() {
    try {
      const { params } = this.$route
      const company = await this.$axios.$get(`/companies/${params.id}`)
      this.company = company
    } catch (error) {
      this.$nuxt.error(error)
    }
  },
  computed: {
    meanRating: function () {
      if(!this.company.reviews){
        return 0
      }
      let mean = 0
      for(const review of this.company.reviews){
        mean += review.rating
      }
      return mean / this.company.reviews.length
    }
  },
  filters:{
    phone: function(value) {
      let x = value.replace(/\D/g, '').match(/(\d{0,2})(\d{0,2})(\d{0,2})(\d{0,2})(\d{0,2})(\d{0,2})/);
      return '+'+ x[1] + ' ' + x[2] + ' ' + x[3] + ' ' + x[4] + ' ' + x[5] + ' ' + x[6] 
    }
  }
}
</script>

<style lang="scss" scoped>
.page-company__contact-info {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  grid-gap: var(--gutter);
}
</style>
