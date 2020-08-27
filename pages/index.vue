<template>
  <section class="section">
    <Card :content="payload.content"/>
  </section>
</template>

<script>
import Card from "../components/Card";

const payloadStore = 'PayloadStore'

export default {
  components: {Card},
  head () {
    return {
      title: 'Pad'
    }
  },
  data: function () {
    return {
      payload: {
        content: 'Default content',
      }
    }
  },
  created() {
    let payload = this.$route.query.payload

    try {
      let storeString = atob(payload)
      localStorage.setItem(payloadStore, storeString)
    } catch (e) {
      // do nothing, fallback to default values
    }
    let storeString = localStorage.getItem(payloadStore)
    if (storeString) {
      try {
        this.payload = JSON.parse(storeString)
      } catch (e) {
        //
      }
    }
  },
}
</script>
