<template>
    <div class="my-select">

        <input
            class="button"
            readonly
            @blur="inputBlur"
            @click="isOpen = !isOpen"
            @keydown="inputKeyDown"
        />

        <div
            class="options"
            v-if="isOpen"
            @mousedown="optionsMouseDown"
        >
            <div
                class="option"
                v-for="(option, index) in list"
                :key="index"
                @mouseup="select(index)"
            >{{ option }}
            </div>
        </div>

    </div>
</template>

<script>
export default {
    name: 'MySelect',

    data: () => ({
        list: {
            10: 'Вариант №1',
            11: 'Вариант №2',
            12: 'Вариант №3',
        },

        isOpen: false,
        isBlurPrevented: false,
        selectedIndex: null,
    }),

    methods: {
        inputBlur({ target }) {
            if (this.isBlurPrevented) {
                this.isBlurPrevented = false;

                target.focus();
            } else {
                this.isOpen = false;
            }
        },

        inputKeyDown({ code }) {
            switch (code) {
            case 'Escape':
                this.isOpen = false;

                break;
            case 'Space':
                this.isOpen = true;

                break;
            case 'Enter':
                break;
            case 'ArrowDown':
                break;
            case 'ArrowUp':
                break;
            }
        },

        optionsMouseDown() {
            this.isBlurPrevented = true;
        },

        select(index) {
            this.selectedIndex = index;
            this.isOpen = false;
        },
    },
};
</script>

<style lang="scss" scoped>
    .my-select {
        width: 400px;
        height: 70px;
        position: relative;
        cursor: pointer;
        background-color: #fff;
    }

    .button {
        width: 100%;
        height: 100%;
        background-color: inherit;
        color: #a9a9a9;
        border: solid 2px;
        cursor: inherit;
        border-radius: 7px;

        &:focus {
            color: #f99;
        }
    }

    .options {
        position: absolute;
        width: 100%;
        border: solid 2px #f99;
        border-top: none;
        border-radius: 0 0 7px 7px;
        top: 60px;
        background-color: inherit;
    }

    .option {
        padding: 5px 10px;

        &:hover {
            color: #fff;
            background-color: #f99;
        }
    }
</style>
