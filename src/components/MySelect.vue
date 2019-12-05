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
            :class="{ hold: currentIndex === null }"
        >{{ label }}</label>

        <div
            class="value"
            v-if="currentIndex !== null"
        >{{ list[currentIndex] }}</div>

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
    props: {
        list: Array,
        label: String,
        currentIndex: Number,
    },
    data: () => ({
        onFocus: false,
        isOpen: false,
        isBlurPrevented: false,
        // TODO сделать обработку null как -1
        targetIndex: null,
    }),
    model: {
        prop: 'currentIndex',
        event: 'change',
    },
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
                this.targetIndex = this.currentIndex;
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
                this.targetIndex = this.currentIndex;

                break;
            case 'Space':
                this.isOpen = true;

                break;
            case 'Enter':
                this.isOpen = false;
                this.$emit('change', this.targetIndex);

                break;
            case 'ArrowDown':
                const needReset = this.targetIndex === null
                    || (++this.targetIndex >= this.list.length);

                if (needReset) this.targetIndex = 0;

                if (!this.isOpen) this.$emit('change', this.targetIndex);

                break;
            case 'ArrowUp':
                if (--this.targetIndex < 0)
                    this.targetIndex = this.list.length - 1;

                if (!this.isOpen) this.$emit('change', this.targetIndex);

                break;
            }
        },
        optionsMouseDown() {
            this.isBlurPrevented = true;
        },
        select(index) {
            this.$emit('change', index);
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