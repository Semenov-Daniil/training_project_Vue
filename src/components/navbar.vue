<template>
    <div class="navbar">
        <div @click="$router.push('/')" >Vue3</div>
        <div class="navbar-btns">
            <my-button @click="$router.push('/posts')" class="navbar-btn">Посты</my-button>
            <my-button @click="$router.push('/about')" class="navbar-btn">О нас</my-button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'navbar',
}
</script>

<style>
    .navbar {
        height: 50px;
        background: gray;
        box-shadow: 2px 2px 4px gray;
        display: flex;
        align-items: center;
        padding: 0 15px;
    }

    .navbar-btns {
        margin-left: auto;
    }

    .navbar-btn {
        margin: 15px;
    }

</style>