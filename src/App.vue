<template>
    <div id="app">
        <MySelect/>
    </div>
</template>

<script>
import MySelect from './components/MySelect';

export default {
    name: 'app',
    components: {
        MySelect,
    },
};
</script>

<style lang="scss">
    * {
        outline: none;
        box-sizing: border-box;
        font-family: sans-serif;
        font-size: 20px;
    }

    body {
        margin: 0;
    }

    #app {
        width: 100vw;
        height: 30vh;
        display: flex;
        justify-content: center;
        align-items: center;
    }
</style>
