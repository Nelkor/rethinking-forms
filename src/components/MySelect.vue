<template>
    <div
        class="my-select"
        :class="{ focus: onFocus }"
    >
        <input
            :id="id"
            readonly
            @focus="onFocus = true"
            @blur="inputBlur"
            @click="isOpen = !isOpen"
            @keydown="inputKeyDown"
        />

        <label
            :for="id"
            :class="{ hold: selectedIndex == -1 }"
        >{{ label }}</label>

        <div
            class="value"
            v-if="selectedIndex != -1"
        >{{ list[selectedIndex] }}</div>

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
                @mousemove="targetIndex = index"
                :class="{ hover: targetIndex == index }"
            >{{ option }}
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'MySelect',

    data: () => ({
        list: [
            'Вариант №1',
            'Вариант №2',
            'Вариант №3',
        ],
        label: 'Вариант',

        onFocus: false,
        isOpen: false,
        isBlurPrevented: false,
        selectedIndex: -1,
        targetIndex: -1,
    }),

    computed: {
        id() {
            return 'input-' + this['_uid'];
        },
    },

    methods: {
        inputBlur(e) {
            if (this.isBlurPrevented) {
                this.isBlurPrevented = false;

                e.target.focus();
            } else {
                this.onFocus = false;
                this.isOpen = false;
                this.targetIndex = this.selectedIndex;
            }
        },

        inputKeyDown(e) {
            switch (e.code) {
            case 'Tab':
                if (this.isOpen) {
                    e.preventDefault();
                }

                break;
            case 'Escape':
                this.isOpen = false;
                this.targetIndex = this.selectedIndex;

                break;
            case 'Space':
                this.isOpen = true;

                break;
            case 'Enter':
                this.isOpen = false;
                this.selectedIndex = this.targetIndex;

                break;

            // Логика обработчиков стрелок похожа.
            // TODO Было бы здорово вынести в функцию.
            case 'ArrowDown':
                if (++this.targetIndex >= this.list.length)
                    this.targetIndex = 0;

                if (!this.isOpen) {
                    if (++this.selectedIndex >= this.list.length)
                        this.selectedIndex = 0;
                }

                break;
            case 'ArrowUp':
                if (--this.targetIndex < 0)
                    this.targetIndex = this.list.length - 1;

                if (!this.isOpen) {
                    if (--this.selectedIndex < 0)
                        this.selectedIndex = this.list.length - 1;
                }

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
        font-size: 20px;

        &.focus {
            input, label {
                color: #f99;
            }
        }
    }

    input {
        width: 100%;
        height: 100%;
        background-color: inherit;
        color: #a9a9a9;
        border: solid 2px;
        cursor: inherit;
        border-radius: 7px;
    }

    label {
        position: absolute;
        color: #a9a9a9;
        left: 6px;
        top: -8px;
        padding: 0 4px;
        background-color: #fff;
        font-size: 16px;
        transition: .2s;

        &.hold {
            top: 20px;
            font-size: 26px;
        }
    }

    .value {
        position: absolute;
        left: 10px;
        top: 20px;
        font-size: 26px;
        transition: .2s;
        pointer-events: none;
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

        &.hover {
            color: #fff;
            background-color: #f99;
        }
    }
</style>
