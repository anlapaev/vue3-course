<template>
    <div class="post">
        <div>
            <div>
                <strong>ID:</strong>
                <p class="p-text">{{ post.id }}</p>
            </div>
            <div>
                <strong>Название:</strong>
                <p class="p-text">{{ post.title }}</p>
            </div>
            <div>
                <strong>Описание:</strong>
                <p class="p-text">{{ post.body }}</p>
            </div>
        </div>
        <div class="post__btns">
            <my-button @click="$router.push('/posts/' + post.id)"
                >Открыть</my-button
            >
            <my-button @click="$emit('remove', post)">Удалить</my-button>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        post: {
            type: Object,
            required: true,
        },
    },
};
</script>

<style scoped>
.post {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 15px;
    border: 2px solid teal;
    margin-top: 15px;
    border-radius: 5px;
    font-size: 20px;
    font-weight: 700;
    color: teal;
    background: rgba(0, 0, 0, 0.05);
    transition: all 0.3s ease;
}
.p-text {
    margin: 0;
    padding: 0;
    font-weight: 500;
    color: rgb(3, 164, 164);
}
</style>
