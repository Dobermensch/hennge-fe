<template>
  <div class="main-view">
    <Datepicker />
    <Mail />
  </div>
</template>

<script>
import Datepicker from "@/components/Datepicker.vue"
import Mail from "@/components/Mail.vue"

export default {
  name: "Main",
  components: {
    Datepicker,
    Mail
  }
}
</script>

<style>
.main-view {
  min-height: 100vh;
  padding-top: 20px;
  padding-left: 45px;
  padding-right: 45px;
  border: 1px solid red;
  position: relative;
}

@media only screen and (max-width: 600px) {
  .main-view {
    padding-top: 10px;
    padding-left: 0px;
    padding-right: 0px;
  }
}
</style>