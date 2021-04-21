<template>
  <VCard class="va-company-review" tag="li">
    <VCardText> I'm an unfinished review item component </VCardText>
  </VCard>
</template>

<script>
export default {
  name: `va-company-review`,
  props: {
    review: { type: Object, default: () => ({}) },
  },
}
</script>
