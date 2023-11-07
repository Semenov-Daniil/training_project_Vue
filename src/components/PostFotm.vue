<template>
  <form @submit.prevent>
    <h1>Создание поста:</h1>
    <my-input 
        type="text" 
        placeholder="Описание"
        v-model="post.title"
    />
    <my-input 
        type="text" 
        placeholder="Название"
        v-model="post.body"
    />
    <my-button
        style="align-self: flex-end; margin-top: 15px;"
        @click="createPost"
    >
    Создать
    </my-button>
  </form>
</template>

<script>
// import myButton from './UI/myButton.vue';
export default {
//   components: { myButton },
    data() {
        return {
            post: {
                title: '',
                body: '',
            }

        }
    },
    methods: {
        createPost() {
            this.post.id = Date.now();
            this.$emit('create', this.post);
            this.post = {
                title: '',
                body: '',
            }
        }
    }
}
</script>

<style>
    form {
        display: flex;
        flex-direction: column;
    }



</style>