<template>
    <button class="btn">
        <slot></slot>
    </button>
</template>

<script>
    export default{
        name: "MyButton"
    }
</script>

<style scoped>
    .btn {
        padding: 10px 15px;
        background-color: white;
        color: teal;
        border: 1px solid teal;
        border-radius: 12px;
    }
</style>