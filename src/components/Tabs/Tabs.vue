<template>
    <div class="tabs-container">
        <div
            class="tabs-button-div"
            v-for="item in tabs"
            :key="item.id"
            :class="{ selected: item.selected }"
        >
            <div
                class="pointer tab-button"
                v-bind:class="item.selected ? 'selected' : 'not-selected'"
                @click="handleClick(item)"
                :disabled="disabled"
            >
                {{ item.name }}
            </div>
        </div>
    </div>
    <slot />
</template>

<script>
export default {
    name: "TabsComponent",
    emits: ["tabChanged"],
    data() {
        return {
            tabs: [],
        };
    },
    props: {
        items: {
            type: Array, // [{'id': 1, 'name': 'tab1', 'selected': true},...]
            required: true,
        },
        disabled: {
            type: Boolean,
            required: false,
            default: false,
        },
        animationPeriod: {
            type: Number,
            default: () => 500,
        },
    },
    mounted() {
        this.tabs = this.items;
    },
    watch: {
        items: function (newVal, oldVal) {
            this.tabs = newVal;
        },
    },
    methods: {
        handleClick(item) {
            if (this.disabled) {
                return;
            }

            if (!item.selected) {
                item.selected = !item.selected;
                this.tabs = this.tabs.map((tab) => {
                    if (item.id == tab.id) {
                        tab.selected = item.selected;
                    } else {
                        tab.selected = !item.selected;
                    }
                    return tab;
                });
            }

            this.$emit("tabChanged", item);
        },
    },
};
</script>

<style lang="scss" scoped>
.tab-button:disabled:hover {
    cursor: not-allowed;
}
.tabs-container {
    display: flex;
    flex-direction: row;
    margin: 40px;
    gap: 32px;
}
.tab-button {
    font-size: 20px;
    cursor: pointer;
}
.tab-button.selected {
    border-bottom: dashed 1px #ccc;
}
</style>
