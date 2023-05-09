<template>
    <div>
        <h1>Page with posts</h1>
        <MyInput
        v-model="searchQuery"
        placeholder="Search..."
        />
        <div class="app__btns">
            <MyButton
            @click="showDialog"
            >
                Create a post
            </MyButton>
            <MySelect
            v-model="selectedSort"
            :options="sortOptions"
            />
        </div>
        
        <MyDialog v-model:show="dialogVisible">
            <PostForm
                @create="createPost"
            />
        </MyDialog>
        
        <PostList 
        :posts="sortedAndSearchedPosts"
        @remove="removePost"
        v-if="!isPostsLoading"
        />
        <div v-else>Loading...</div>
        <div v-intersection="loadMorePosts"></div>
        <!-- <div class="page__wrapper">
            <div v-for="curPage in totalPages" :key="page"
            class="page"
            :class="{
                'current__page': curPage === page
            }"
            @click="changePage(curPage)"
            >
                {{ curPage }}
            </div>
        </div> -->
    </div>
</template>

<script>
import PostForm from '@/components/PostForm.vue';
import PostList from '@/components/PostList.vue';
import axios from "axios";

export default {
    components: {
        PostForm, PostList
    },

    data() {
        return {
            posts: [],
            dialogVisible: false,
            isPostsLoading: false,
            selectedSort: '',
            searchQuery: '',
            page: 1,
            limit: 10,
            totalPages: 0,
            sortOptions: [
                {value: 'title', name: 'By name'},
                {value: 'body', name: 'By descr'}
            ]
        }
    },
    methods: {
        createPost(post) {
            this.posts.push(post);
            this.dialogVisible = false;
        },
        removePost(post){
            this.posts = this.posts.filter(p => p.id !== post.id);
        },
        showDialog(){
            this.dialogVisible = true;
        },
        async fetchPosts() {
            try {
                this.isPostsLoading = true;
                const response = await axios.get("https://jsonplaceholder.typicode.com/posts",
                {
                    params: {
                        _page: this.page,
                        _limit: this.limit
                    }
                });
                this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit);
                this.posts = response.data;
                this.isPostsLoading = false;
            }
            catch(e) {
                alert(e.name + " " + e.message);
            }
            finally {
                this.isPostsLoading = false;
            }
        },
        // changePage(curPage) {
        //     this.page = curPage;
        // },
        async loadMorePosts() {
            try {
                this.page += 1;
                const response = await axios.get("https://jsonplaceholder.typicode.com/posts",
                {
                    params: {
                        _page: this.page,
                        _limit: this.limit
                    }
                });
                this.posts = [...this.posts, ...response.data];
            }
            catch(e) {
                alert(e.name + " " + e.message);
            }
        },
        
    },
    mounted() {
        this.fetchPosts();
    },
    computed: {
        sortedPosts() {
            return [...this.posts].sort((post1, post2) =>
                post1[this.selectedSort]?.localeCompare(post2[this.selectedSort])
            );
        },
        sortedAndSearchedPosts() {
            return this.sortedPosts.filter(post => 
            post.title.toLowerCase().includes(this.searchQuery.toLowerCase()));
        }
    },
    watch: {
        // page() {
        //     this.fetchPosts();
        // }
    }
}
</script>

<style>
    

    .app__btns {
        margin: 15px 0px;
        display: flex;
        justify-content: space-between;
    }    
    
    .page__wrapper {
        display: flex;
        margin-top: 15px;
    }

    .page {
        border: 1px solid black;
        padding: 10px;
    }

    .current__page {
        border: 2px solid teal;
    }
</style>
