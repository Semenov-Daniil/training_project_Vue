<template>
    <navbar/>
    <div class="app">
        <router-view></router-view>
    </div>
</template>

<script>
import navbar from "@/components/navbar.vue";

export default {
    components: {
        navbar
    },
}
</script>

<style>
    body {
        margin: 0;
    }

    .app {
        padding: 15px;
        display: flex;
        flex-direction: column;
    }
</style>