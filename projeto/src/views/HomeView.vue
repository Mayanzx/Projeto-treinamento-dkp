<template>
  <Banner />
  <div class="main-container">
    <h1>Cadastre o seu time:</h1>
    <TeamForm />
  </div>
</template>

<script>
  import Banner from "../components/Banner.vue"
  import TeamForm from "../components/TeamForm.vue"

  export default {
    name: "Home",
    components: {
      Banner,
      TeamForm
    }
  }
</script>
