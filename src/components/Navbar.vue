<template>
    <div class="navbar">
        <div class="navbar__logo" @click="$router.push('/')">Vue 3</div>
        <div class="navbar__btns">
            <my-button @click="$router.push('/posts')">Посты</my-button>
            <my-button @click="$router.push('/about')">О сайте</my-button>
            <my-button @click="$router.push('/store')">Store</my-button>
        </div>
    </div>
</template>

<script>
import MyButton from './UI/MyButton.vue';
export default {
  components: { MyButton },};
</script>

<style scoped>
.navbar {
    height: 90px;
    background-color: lightgray;
    box-shadow: 2px 2px 4px gray;
    display: flex;
    align-items: center;
    padding: 0 15px;
}

.navbar__logo {
    font-weight: bold;
    font-size: 24px;
    cursor: pointer;
}

.navbar__btns {
    margin-left: auto;
}
</style>
