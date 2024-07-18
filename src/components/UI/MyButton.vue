<template>
    <button class="btn">
        <slot></slot>
    </button>
</template>

<script>
    export default {
        name: "my-button",
    }
</script>

<style  scoped>
.btn {
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
}
</style>