<template>
    <div>
        <p>{{screenOutput}}</p>
    </div>
</template>

<script>
export default {
    props: ['screenOutput'],

    data() {
        return {
            output: '0',
        }
    }
}
</script>

<style scoped>
div {
    width: 100%;
    height: 10%;
    text-align: right;
    padding-top: 0.3rem;
    padding-right: 0.8rem;
    border-radius: 10px;
    background-color: rgba(235, 213, 157, 0.356);
}

p {
    font-size: 28px;
}
</style>