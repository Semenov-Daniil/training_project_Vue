<template>
    <div 
        class="dialog"
        v-show="show"
        @click.stop="hideDialog"
    >
        <div @click.stop class="dialog_content">
            <slot>

            </slot>
        </div>
    </div>
</template>

<script>
export default {
    name: 'my-dialog',
    props: {
        show: {
            type: Boolean,
            default: false
        }
    },
    methods: {
        hideDialog() {
            this.$emit('update:show', false);
        }
    }
}
</script>

<style>
    .dialog {
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        background: rgba(0, 0, 0, 0.5);
        position: absolute;
        display: flex;
    }

    .dialog_content {
        margin: auto;
        background: white;
        border-radius: 12px;
        min-height: fit-content;
        min-width: 50vw;
        padding: 20px;
    }
</style>