<template>
    <div id="app">
        <MySelect
            label="Роль"
            :list="roles"
            v-model="currentRole"
        />
    </div>
</template>

<script>
import MySelect from './components/MySelect';

export default {
    name: 'app',
    data: () => ({
        roles: [
            'Лёгкая',
            'Центр',
            'Сложная',
            'Поддержка',
            'Полная поддержка',
        ],
        currentRole: null,
    }),
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
