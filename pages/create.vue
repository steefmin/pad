<template>
  <section class="section">
    <b-field>
      <b-input
        type="textarea"
        v-model="payload.content"
        required
      />
    </b-field>
    <div class="buttons">
      <b-button type="is-success" icon-left="check" @click="save()">Save</b-button>
      <b-button type="is-info" icon-left="share" @click="isLinkActive = true" :disabled="isLinkButtonDisabled">Share link</b-button>
      <b-button type="is-danger" icon-left="redo" @click="clear()">Clear</b-button>
    </div>
    <b-message auto-close title="Link" type="is-success" has-icon :active.sync="isLinkActive"
               aria-close-label="Close message" :duration=10000
    >
      <a :href="getPayloadLink()" target="_blank">{{ getPayloadLink() }}</a>
    </b-message>
  </section>
</template>

<script>
import Card from '~/components/Card'

const payloadStore = 'PayloadStore'

export default {
  components: {
    Card,
  },
  data() {
    return {
      isLinkActive: false,
      payload: {
        content: '',
      }
    }
  },
  head() {
    return {
      title: 'Pad'
    }
  },
  computed: {
    isLinkButtonDisabled() {
      if (this.isLinkActive) {
        return true
      }
      if (this.payloadIsDefault()) {
        return true
      }
      return false
    },
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
  methods: {
    payloadIsDefault() {
      if (this.payload.content === '') {
        return true
      }
      return false
    },
    query() {
      const payload = JSON.stringify(this.payload)
      return `?payload=${btoa(payload)}`
    },
    getPayloadLink() {
      return `${window.location.origin}/${this.query()}`
    },
    save() {
      localStorage.setItem(payloadStore, JSON.stringify(this.payload))
      this.$buefy.toast.open({message: 'Saved', type: 'is-success'})
    },
    clear() {
      localStorage.removeItem(payloadStore)
      this.payload = {
        title: '',
        content: '',
        imageUrl: null
      }
      this.$buefy.toast.open({message: 'Form cleared', type: 'is-success'})
    },
  }
}
</script>


<style scoped>
.section {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

</style>
