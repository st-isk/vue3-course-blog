<template>
    <div class="navbar">
        <div @click="$router.push('/')">Test vue app</div>
        <div class="navbar__btns">
            <MyButton @click="$router.push('/posts')">Posts</MyButton>
        </div>
    </div>
</template>

<script>

export default {
    
}
</script>

<style scoped>
    .navbar {
        height: 50px;
        background-color: teal;
        box-shadow: 2px 2px 4px gray;
        display: flex;
        align-items: center;
        padding: 0 15px;
    }

    .navbar__btns {
        margin-left: auto;
    }
</style>