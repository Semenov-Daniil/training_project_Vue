<template>
    <div class="post">
        <div>
            <div>ID: {{ post.id }}</div>
            <div>Название: {{ post.title }}</div>
            <div>Описание: {{ post.body }}</div>
        </div>
        <div class="post_btns">
            <my-button
                @click="$router.push(`/posts/${post.id}`)"
            >
            Открыть
            </my-button>
            <my-button
                @click="$emit('remove', post)"
            >
            Удалить
            </my-button>
        </div>
    </div>
</template>

<script>
import myButton from './UI/myButton.vue'
// import myButton from './UI/myButton.vue'
export default {
  components: { myButton },
//   components: { myButton },
    props: {
        post: {
            type: Object,
            required: true
        }
    }
}
</script>

<style>
    .post {
        padding: 15px;
        margin-top: 15px;
        border: 2px solid teal;

        display: flex;
        align-items: center;
        justify-content: space-between;
    }

    .post_btns {
        display: flex;
        gap: 10px;
    }
</style>