<template>
    <div class="conteiner">
        <!-- <div>
            Кол-во лайков: {{ likes }}
            <button @click="likes++">+</button>
            <button @click="likes--">-</button>
        </div>
        <div>
            Кол-во дизлайков: {{ dislikes }}
            <button @click="dislikes++">+</button>
            <button @click="dislikes--">-</button>
        </div> -->

        
        <h1>Страница с постами</h1>

        <my-input
            v-model="searchQuery"
            placeholder="Поиск..."
        />

        <div class="app_btns">
            <my-button
                @click="showDialog"
            >
                Создать пост
            </my-button>
            <my-select
                v-model="selectedSort"
                :options="sortOptions"
            />
        </div>

        <my-dialog
            v-model:show="dialogVisible"
        >
            <post-form 
                @create="createPost"    
            />
        </my-dialog>
        <post-list 
            :posts="sortedAndSearchedPosts"
            @remove="removePost"
            v-if="!isPostsLoading"

        />
        <div v-else style="margin: 15px;">Идет загрузка...</div>

        <div ref="observer" class="observer">

        </div>

        <!-- <div class="page_wrapper">
            <div 
                v-for="pageNumber in totalPage" 
                :key="pageNumber"
                class="page"
                :class="{
                    'current-page': page === pageNumber
                }"
                @click="changePage(pageNumber)"
            >
                {{ pageNumber }}
            </div>
        </div> -->

        <my-page
            :totalPage="totalPage"
            v-model="page"
            @update="changePage"
        />
    </div>
</template>

<script>
import PostForm from "@/components/PostFotm.vue";
import PostList from "@/components/PostList.vue";
import MyPage from '@/components/UI/myPage.vue';

export default {
    data() {
        return {
            likes: 0,
            dislikes: 0,
            posts: [],
            dialogVisible: false,
            isPostsLoading: false,
            selectedSort: "",
            sortOptions: [
                {value: 'title', name: 'По названию'},
                {value: 'body', name: 'По описанию'},
            ],
            searchQuery: '',
            page: 1,
            limit: 10,
            totalPage: 0
        }
    },
    methods: {
        createPost(post) {
            this.posts.push(post);
            this.dialogVisible = false;
        },
        removePost(post) {
            this.posts = this.posts.filter(p => p.id !== post.id);
        },
        showDialog() {
            this.dialogVisible = true;
        },
        async fetchPosts() {
            this.isPostsLoading = true;
            try {
                let response = await fetch('https://jsonplaceholder.typicode.com/posts' + '?' + new URLSearchParams({ _page: this.page, _limit: this.limit }));   
                this.totalPage = Math.ceil(response.headers.get('x-total-count') / this.limit);
                this.posts = await response.json();
            } catch (err) {
                alert(err);
            } finally {
                this.isPostsLoading = false
            }
            // fetch('https://jsonplaceholder.typicode.com/posts' + '?' + new URLSearchParams({ _page: this.page, _limit: this.limit }))
            //     .then((response) => { 
            //         this.totalPage = Math.ceil(response.headers.get('x-total-count') / this.limit);
            //         return response.json() 
            //     })
            //     .then(data => this.posts = data)
            //     .catch(err => alert(err))
            //     .finally(this.isPostsLoading = false);
        },
        changePage(pageNumber) {
            this.page = pageNumber;
        },
        async loadMorePosts() {
            this.page += 1;
            try {
                let response = await fetch('https://jsonplaceholder.typicode.com/posts' + '?' + new URLSearchParams({ _page: this.page, _limit: this.limit }));   
                this.totalPage = Math.ceil(response.headers.get('x-total-count') / this.limit);
                this.posts = [...this.posts, ...(await response.json())];
            } catch (err) {
                alert(err);
            } finally {
                this.isPostsLoading = false
            }
            // fetch('https://jsonplaceholder.typicode.com/posts' + '?' + new URLSearchParams({ _page: this.page, _limit: this.limit }))
            //     .then((response) => { 
            //         this.totalPage = Math.ceil(response.headers.get('x-total-count') / this.limit);
            //         return response.json() 
            //     })
            //     .then(data => this.posts = [...this.posts, ...data])
            //     .catch(err => alert(err));
        },
    },
    components: {
        PostForm, PostList,
        MyPage
    },
    mounted() {
        this.fetchPosts();
        // let options = {
        //     rootMargin: "0px",
        //     threshold: 1.0,
        // };
        // let callback = (entries, observer) => {
        //     if (entries[0].isIntersecting && this.page < this.totalPage) {
        //         this.loadMorePosts();
        //     }
        // };
        // let observer = new IntersectionObserver(callback, options);
        // observer.observe(this.$refs.observer);
    },
    watch: {
        // selectedSort(newValue) {
        //     this.posts.sort((post1,post2) => {
        //         return post1[newValue]?.localeCompare(post2[newValue]);
        //     })
        // },
        // page() {
        //     this.fetchPosts();
        // }
    },
    computed: {
        sortedPosts() {
            return [...this.posts].sort((post1,post2) => {
                return post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]);
            })
        },
        sortedAndSearchedPosts() {
            return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()));
        }
    }
}
</script>

<style>


    .app_btns {
        margin: 15px 0;
        display: flex;
        justify-content: space-between;
    }

    .observer {
        height: 30px;
        background: green;
    }

    .conteiner {
        display: flex;
        flex-direction: column;
    }

</style>