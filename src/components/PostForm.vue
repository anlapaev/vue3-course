<template>
    <form class="form-post" action="" @submit.prevent>
        <h4>Создание поста</h4>
        <my-input
            
            v-focus
            v-model="post.title"
            type="text"
            placeholder="Название поста"
        />
        <p>{{ post }}</p>
        <my-input
            v-model="post.body"
            type="text"
            placeholder="Описание поста"
        />
        <my-button @click="createPost">Создать</my-button>
    </form>
</template>

<script>
import MyInput from "./UI/MyInput.vue";
export default {
    components: { MyInput },
    data() {
        return {
            post: {
                title: "",
                body: "",
            },
        };
    },
    methods: {
        createPost() {
            this.post.id = Date.now();
            this.$emit("create", this.post);
            this.post = {
                title: "",
                body: "",
            };
        },
    },
};
</script>

<style scoped>
.form {
    display: flex;
    flex-direction: column;
    max-width: 500px;
    margin: 0 auto;
    padding: 15px;
    border: 2px solid teal;
    margin-top: 15px;
    border-radius: 5px;
    align-items: center;
    justify-content: center;
    gap: 15px;
    text-align: center;
    font-size: 20px;
    font-weight: 700;
    color: teal;
    background: rgba(0, 0, 0, 0.05);
    transition: all 0.3s ease;
}

.form-post {
    display: flex;
    flex-direction: column;
    max-width: 500px;
    margin: 0 auto;
    padding: 15px;
    border: 2px solid teal;
    margin-top: 15px;
    border-radius: 5px;
    align-items: center;
    justify-content: center;
    gap: 15px;
    text-align: center;
    font-size: 20px;
    font-weight: 700;
    color: teal;
    background: rgba(0, 0, 0, 0.05);
    transition: all 0.3s ease;
}

/* .container {
    padding: 20px;
    margin: 0 auto;
    max-width: 500px;
} */
</style>
