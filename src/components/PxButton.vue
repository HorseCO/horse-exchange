<template>
    <button @click="buttonClick" class="bg-transparent hover:bg-green-500 text-green-700 font-semibold">
        <beat-loader :loading="isLoading" :color="'#68d391'" :size="100" />
        <p v-show="!isLoading">
            <slot></slot>
        </p>
    </button>
</template>

<script>
export default {
    name: 'PxButton',
    props:{
        isLoading: {
            type: Boolean,
            default: false
        }        
    },
    methods: {
        buttonClick(){
            this.$emit('custom-click');
        }
    }
}
</script>