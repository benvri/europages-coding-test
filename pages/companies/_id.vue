<template>
  <div class="page-company">
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
        <VAlert type="info" class="mx-4 d-inline-block">
          TODO: MINOR – display the mean rating
          <br />
          <a
            href="https://vuetifyjs.com/en/components/ratings/"
            target="_blank"
            rel="noopener noreferrer"
            class="white--text"
          >
            Use the Vuetify Rating component
          </a>
          <br />
          Remove the <code>VAlert</code> when it's done
        </VAlert>
      </VImg>
      <VCardText>
        <div class="page-company__contact-info">
          <VaCompanyLocation :location="company.location" />
          <div>
            <PhPhone size="24" />
            <p class="text-body-1">
              {{ company.phone }}
              <VAlert text>
                ↑ TODO: PATCH – we would like to display the formatted phone<br />
                There is a very simple solution to that the Backend dev said
                <br />
                Remove the <code>VAlert</code> when it's done
              </VAlert>
            </p>
          </div>
        </div>
      </VCardText>
    </VCard>
    <aside>
      <VAlert text>
        TODO: PATCH – this should go under the company card on small device
        <br />
        Remove the <code>VAlert</code> when it's done
      </VAlert>
      <VAlert text type="error">
        TODO: MAJOR – implement the reviews<br />
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
      <ul class="pa-0">
        <VSkeletonLoader
          v-if="$fetchState.pending"
          type="article"
          elevation="2"
        />
        <VaCompanyReview v-else />
      </ul>
    </aside>
  </div>
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
    // TODO: we should be able to have the mean of all ratings
  },
}
</script>

<style lang="scss" scoped>
.page-company {
  display: grid;
  grid-template-columns: minmax(0, 1fr) 16rem;
  grid-gap: var(--gutter);
}
.page-company__contact-info {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  grid-gap: var(--gutter);
}
</style>
