<template>
    <a
        class="
          bg-indigo-700
          hover:bg-indigo-500
          text-white
          ml-4
          py-2
          px-3
          rounded-lg
          cursor-pointer
        "
        @click="onClickButton"
    >
        {{ title }}
    </a>
</template>
<script>
export default {
    props: {
        title: {
            default: '',
            type: String
        }
    },
    methods: {
        onClickButton () {
            this.$emit('onClickButton');
        }
    }
}
</script>