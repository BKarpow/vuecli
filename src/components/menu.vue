<template>
    <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
        <a class="navbar-brand" href="">
            <slot></slot>
        </a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>

        <div class="collapse navbar-collapse" id="navbarSupportedContent">
            <ul class="navbar-nav mr-auto">

<!--                    <a class="nav-link" href="#">Home <span class="sr-only">(current)</span></a>-->

                    <router-link
                            v-for="link in menuLinks"
                            :key="link.path"
                            :to="link.path"
                            v-slot="{ href, route, navigate, isActive, isExactActive }"
                    >
                        <li class="nav-item" :class="{'active': isActive}">
                    <a
                            :href="href"
                            @click="navigate"
                            class="nav-link"
                            :data-fulpath="route.fullPath"
                            :data-isactive="isActive"
                            :class="[ isExactActive && 'active']"
                    >{{link.name}}</a>
                        </li>
                </router-link>

            </ul>
            <form class="form-inline my-2 my-lg-0">
                <input class="form-control mr-sm-2" type="search" placeholder="Search" aria-label="Search">
                <button class="btn btn-outline-success my-2 my-sm-0" type="submit">Search</button>
            </form>
        </div>
    </nav>
</template>
<script>
    export default {
        name: 'Menu',
        data(){
            return {
                menuLinks: {}
            }
        },
        mounted() {
            // console.log('test', this.$router.getRoutes())
            this.menuLinks = this.$router.getRoutes()
        }
    }
</script>