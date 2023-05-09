<template>
    <div class="post">
        <div>
            <div>Name: {{ post.title }}</div>
            <div>Descr: {{ post.body }}</div>
        </div>
        <div class="post__btns">
            <MyButton @click="$router.push(`/posts/${post.id}`)">
                Open
            </MyButton>
            <MyButton style="margin-left: 5px;" @click="$emit('remove', post)">
                Delete
            </MyButton>
        </div>
    </div>
</template>

<script>

export default {
    props: {
        post: {
            type: Object,
            required: true
        }
    }
}
</script>

<style scoped>
    .post {
        padding: 15px;
        border: 2px solid teal;
        margin: 15px;
        display: flex;
        align-items: center;
        justify-content: space-between;
        border-radius: 12px;
    }

    .post__btns {
        margin-left: 5px;
        display: flex;
    }
</style>