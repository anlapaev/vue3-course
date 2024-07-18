<template>
    <div class="container">
        <div class="app__btns">
            <my-button @click="showDialog">Создать пост</my-button>
            <my-select v-model="selectedSort" :options="sortOptions">
            </my-select>
        </div>
        <h1>Список постов</h1>

        <my-dialog v-model:show="dialogVisible">
            <post-form @create="createPost"></post-form>
        </my-dialog>

        <post-list
            :posts="sortedPosts"
            @remove="removePost"
            v-if="!isPostsLoading"
        ></post-list>
        <div v-else class="loader">Загрузка...</div>
    </div>
</template>

<script>
import PostList from "@/components/PostList.vue";
import PostForm from "@/components/PostForm.vue";
import MyDialog from "./components/UI/MyDialog.vue";
import MyButton from "./components/UI/MyButton.vue";
import axios from "axios";

export default {
    components: {
        PostList,
        PostForm,
        MyDialog,
        MyButton,
    },
    data() {
        return {
            posts: [],
            dialogVisible: false,
            isPostsLoading: false,
            selectedSort: "",
            sortOptions: [
                { value: "title", name: "По названию" },
                { value: "body", name: "По содержанию" },
            ],
        };
    },
    methods: {
        createPost(post) {
            this.posts.push(post);
            this.dialogVisible = false;
        },

        removePost(post) {
            this.posts = this.posts.filter((p) => p.id !== post.id);
        },

        showDialog() {
            this.dialogVisible = true;
        },
        async fetchPosts() {
            try {
                this.isPostsLoading = true;
                setTimeout(async () => {
                    const response = await axios.get(
                        "https://jsonplaceholder.typicode.com/posts?_limit=10"
                    );
                    this.posts = response.data;
                    this.isPostsLoading = false;
                }, 1);
            } catch (e) {
                alert(e);
            }
        },
    },
    mounted() {
        this.fetchPosts();
    },
    computed: {
        sortedPosts() {
            return [...this.posts].sort((post1, post2) => {
                return post1[this.selectedSort]?.localeCompare(
                    post2[this.selectedSort]
                );
            });
        },
    },
    // watch : {
    //     selectedSort(newValue) {
    //         this.posts.sort((post1, post2) => {
    //             return post1[newValue]?.localeCompare(post2[newValue]);
    //         });
    //     }
    // }
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

.btn {
    margin-top: 15px;
    align-self: flex-end;
    padding: 10px 15px;
    background: none;
    color: teal;
    border: 1px solid teal;
    cursor: pointer;
    transition: all 0.3s ease;
    border-radius: 5px;
    font-size: 20px;
    font-weight: 700;
    color: teal;
    background: rgba(0, 0, 0, 0.05);
    transition: all 0.3s ease;
}
.btn:hover {
    color: white;
    background: teal;
    border: 1px solid teal;
}

.loader {
    text-align: center;
    font-size: 30px;
}

.app__btns {
    margin: 15px 0;
    display: flex;
    justify-content: space-between;
}
</style>
