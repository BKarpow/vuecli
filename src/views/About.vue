<template>
  <div class="about">
    <h1>This is an about page</h1>
    <h2>Your token {{token}}</h2>
  </div>
</template>

<script>
  import Cookies from 'js-cookie'
  export default {
    name: "About",
    data(){
      return {
        token: null
      }
    },
    mounted() {
      // get token from cookie uid
      this.token = Cookies.get('uid')
    }
  }
</script>
