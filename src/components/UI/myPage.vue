<template>
    <div class="page_wrapper">
        <div 
            v-for="pageNumber in totalPage" 
            :key="pageNumber"
            class="page"
            :class="{
                'current-page': modelValue === pageNumber
            }"
            @click="updatePage(pageNumber)"
            :value="modelValue"
        >
            {{ pageNumber }}
        </div>
    </div>
</template>

<script>
export default {
    name: 'my-page',
    props: {
        totalPage: {
            type: Number
        },
        modelValue: {
            type: Number
        }
    },
    methods: {
        updatePage(pageNumber) {
            this.$emit('update:modelValue', pageNumber);
        }
    }
}
</script>

<style>
    .page_wrapper {
        display: flex;
        margin-top: 15px;
        gap: 10px;
        justify-content: center;
        font-size: 25px;
    }

    .page {
        border: 1px solid black;
        padding: 5px 10px;
    }

    .current-page {
        color: teal;
        border: 2px solid teal;
    }
</style>