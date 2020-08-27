<template>
  <div>
    <Navbar/>
    <div class="columns">
      <div class="column"></div>
      <div class="column is-10">
        <section class="main-content section is-medium">
          <nuxt/>
        </section>
      </div>
      <div class="column"></div>
    </div>
  </div>
</template>

<script>
import Navbar from "../components/Navbar";
export default {
  components: {Navbar}
}
</script>
