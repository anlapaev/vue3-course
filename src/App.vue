<template>
    <div class="container">
        <navbar></navbar>
        <router-view></router-view>
    </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
export default {
    components: {
        Navbar,
    },
};
</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

.container {
    max-width: 800px;
    margin: 0 auto;
}
</style>
