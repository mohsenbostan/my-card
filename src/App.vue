<template>
    <div id="app">
        <vs-navbar collapse v-model="activeItem" class="nabarx">
            <div slot="title" style="text-align: center; width: 100%">
                <vs-navbar-title>
                    مای کارت
                </vs-navbar-title>
            </div>

            <vs-navbar-item index="0">
                <router-link to="/home">کارت ها</router-link>
                |
            </vs-navbar-item>
            <vs-navbar-item index="1">
                <router-link to="/about">درباره برنامه</router-link>
            </vs-navbar-item>
        </vs-navbar>
        <div style="margin: 2rem 0;">
            <router-view/>
        </div>
    </div>
</template>
<script>
    export default {
        data: () => ({
            activeItem: 0
        })
    }
</script>
<style>
    #app {
        font-family: 'Vazir FD', Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
    }

    button, input {
        font-family: 'Vazir FD', Tahoma !important;
    }
</style>
