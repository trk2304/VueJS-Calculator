<template>
    <button @click="buttonClick">{{ faceValue }}</button>
</template>

<script>
export default {
    props: {
        faceValue: String
    },

    emits: ['calc-button-click'],

    data() {
        return {
            value: ''
        }
    },

    methods: {
        buttonClick() {
            this.$emit('calc-button-click', this.faceValue);
            console.log('This button was clicked.')
        }
    }
}
</script>

<style scoped>
button {
    width: 100px;
    background-color: lightslategray;
    border: 1px solid lightslategray;
    border-radius: 5px;
    color: white;
    padding: 0.6rem 0.5rem;
    margin: 0 0.3rem;
    font-size: 20px;
    box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px;
    transition: 200ms ease-in-out;
}

button:hover {
    background-color: white;
    color: black;
}
</style>