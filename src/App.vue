<template>
  <div id="app">
    <header>

      <Menu>
        <h1><i class="fas fa-book-open"></i></h1>
      </Menu>
    </header>
    <main class="container mt-3">
      <router-view/>
    </main>
  </div>
</template>

<script>
  import axios from 'axios'
  import Cookies from 'js-cookie'
  import Menu from './components/menu.vue'

  export default {
    data(){
      return {
        response: {},
        errors: []
      }
    },
    components:{
      Menu
    },
    watch:{
      response: function(){
        this.go_login_page()
        if (this.response.auth && this.response.token){
          Cookies.set('uid', this.response.token, {expires: 7})
        }else{
          this.$router.beforeEach((to, from, next) => {
            if (to.path !== '/login' ){
              this.go_login_page()
            }
            next()
          })
        }
      }
    },
    mounted() {
      axios.get('http://localhost/')
        .then(resp => {
          this.response = resp.data
          console.log(resp.data)

        })
      .catch(e => {
        this.error = e
      })

    },
    methods: {
      go_login_page(){
        if (!this.response.auth){
          this.$router.push('/login')
        }
      }
    }
  }
</script>

<style>


#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
