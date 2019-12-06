<template>
    <div class="model-tab">
        <input
            readonly
            ref="input"
            @keydown="onKeyDown"
        >

        <div class="tabs">
            <div
                class="tab"
                v-for="(name, index) in tabs"
                :key="index"
                :class="{ active: index == current }"
                @mousedown.prevent="$refs.input.focus()"
                @click="$emit('change', index)"
            >{{ name }}
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ModelTab',
    props: {
        tabs: Array,
        current: Number,
    },
    methods: {
        onKeyDown({ code }) {
            switch (code) {
            case 'ArrowLeft':
                const chooseFrom = this.current == 0
                    ? this.tabs.length
                    : this.current;

                this.$emit('change', chooseFrom - 1);
                break;
            case 'ArrowRight':
                const next = this.current == (this.tabs.length - 1)
                    ? 0
                    : this.current + 1;

                this.$emit('change', next);
                break;
            }
        },
    },
    model: {
        prop: 'current',
        event: 'change',
    },
};
</script>

<style lang="scss" scoped>
    .model-tab {
        height: 2rem;
        position: relative;
        user-select: none;
    }

    input {
        position: absolute;
        width: 100%;
        height: 100%;
        border: none;

        &:focus {
            background-color: #fcfcff;
        }
    }

    .tabs {
        position: absolute;
        width: 100%;
        height: 100%;
        display: flex;
    }

    .tab {
        font-size: .8rem;
        font-weight: 500;
        padding: 0 2rem;
        display: flex;
        justify-content: center;
        align-items: center;
        border-bottom: .12rem solid #BAC5CB;
        cursor: pointer;

        &.active {
            color: #3874E0;
            border-color: #3874E0;
        }
    }
</style>
