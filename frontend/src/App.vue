<template>
    <div id="app">
        <div v-if="admin || waiter">
            <router-view></router-view>
        </div>
        <div v-else>
            <NavBar />

            <div class="auth-wrapper">
                <div class="auth-inner">
                    <router-view></router-view>
                </div>
            </div>
            <FooterComponent />
        </div>
    </div>
</template>

<script>
import NavBar from './components/NavBar.vue';
import { mapActions } from 'vuex';
import { mapState } from 'vuex';

export default {
    name: 'App',
    components: {
        NavBar
    },

    created() {
        this.getFoodsData()
    },

    computed: {
        ...mapState(["admin", "waiter"])
    },

    methods: {
        ...mapActions(["getFoodsData"])
    }
}
</script>

<style>
@import "./assets/css/global_style.css";
</style>
