<template>
    <div>
        <div class="app__btns">
            <my-button @click="showDialog">Создать пост</my-button>
            <my-select v-model="selectedSort" :options="sortOptions">
            </my-select>
        </div>
        <h1>Список постов</h1>
        <my-input v-focus v-model="searchQuery" placeholder="Поиск ..."> </my-input>
        <my-dialog v-model:show="dialogVisible">
            <post-form @create="createPost"></post-form>
        </my-dialog>

        <post-list
            :posts="sortedAndSearchedPosts"
            @remove="removePost"
            v-if="!isPostsLoading"
        ></post-list>
        <div v-else class="loader">Загрузка...</div>
        <div v-intersection="loadMorePosts" class="observer"></div>
        <!-- <div class="page__wrapper">
          <div
              v-for="page in totalPages"
              :key="page"
              class="page"
              :class="{ 'current-page': currentPage === page }"
              @click="changePage(page)"
          >
              {{ page }}
          </div>
      </div> -->
    </div>
</template>

<script>
import PostList from "@/components/PostList.vue";
import PostForm from "@/components/PostForm.vue";
import MyDialog from "@/components/UI/MyDialog.vue";
import MyButton from "@/components/UI/MyButton.vue";
import axios from "axios";
import MyInput from "@/components/UI/MyInput.vue";
import { watch } from "vue";

export default {
    components: {
        PostList,
        PostForm,
        MyDialog,
        MyButton,
        MyInput,
    },
    data() {
        return {
            posts: [],
            dialogVisible: false,
            isPostsLoading: false,
            selectedSort: "",
            searchQuery: "",
            currentPage: 1,
            limit: 10,
            totalPages: 0,
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

        // changePage(page) {
        //     this.currentPage = page;
        // },
        async fetchPosts() {
            try {
                this.isPostsLoading = true;
                setTimeout(async () => {
                    const response = await axios.get(
                        "https://jsonplaceholder.typicode.com/posts",
                        {
                            params: {
                                _page: this.currentPage,
                                _limit: this.limit,
                            },
                        }
                    );
                    this.totalPages = Math.ceil(
                        response.headers["x-total-count"] / this.limit
                    );
                    this.posts = response.data;
                    this.isPostsLoading = false;
                }, 1);
            } catch (e) {
                alert(e);
            }
        },
        async loadMorePosts() {
            try {
                this.currentPage += 1;
                setTimeout(async () => {
                    const response = await axios.get(
                        "https://jsonplaceholder.typicode.com/posts",
                        {
                            params: {
                                _page: this.currentPage,
                                _limit: this.limit,
                            },
                        }
                    );
                    this.totalPages = Math.ceil(
                        response.headers["x-total-count"] / this.limit
                    );
                    this.posts = [...this.posts, ...response.data];
                }, 1);
            } catch (e) {
                alert(e);
            }
        },
    },
    mounted() {
        this.fetchPosts();
        this.$refs.observer;

        // const options = {
        //     rootMargin: "0px",
        //     threshold: 1.0,
        // };
        // const callback = (entries, observer) => {
        //     if (
        //         entries[0].isIntersecting &&
        //         this.currentPage < this.totalPages
        //     ) {
        //         this.loadMorePosts();
        //     }
        // };
        // const observer = new IntersectionObserver(callback, options);
        // observer.observe(this.$refs.observer);
    },
    computed: {
        sortedPosts() {
            return [...this.posts].sort((post1, post2) => {
                return post1[this.selectedSort]?.localeCompare(
                    post2[this.selectedSort]
                );
            });
        },

        sortedAndSearchedPosts() {
            return this.sortedPosts.filter((post) =>
                post.title
                    .toLowerCase()
                    .includes(this.searchQuery.toLowerCase())
            );
        },

        // watch : {
        //     selectedSort(newValue) {
        //         this.posts.sort((post1, post2) => {
        //             return post1[newValue]?.localeCompare(post2[newValue]);
        //         });
        //     }
        // }
    },

    watch: {
        // currentPage() {
        //     this.fetchPosts();
        // },
    },
};
</script>

<style>
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

.page__wrapper {
    display: flex;
    margin-top: 15px;
    justify-content: center;
}

.page {
    padding: 10px;
    border: 1px solid black;
    cursor: pointer;
}

.current-page {
    border: 2px solid teal;
}

.observer {
    height: 30px;
    background: rgba(255, 255, 255, 0);
}
</style>
