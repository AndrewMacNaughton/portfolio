<template>
    <tabs-component @tabChanged="handleTabChanged" :items="tabs">
        <div class="container">
            <div v-if="selectedTab === 1"><multi-select /></div>
            <div v-if="selectedTab === 2"><grid-view /></div>
        </div>
    </tabs-component>
</template>
<script setup>
import MultiSelect from "@/components/multi/Wrapper.vue";
import GridView from "@/components/grid/Wrapper.vue";
import TabsComponent from "@/components/Tabs/Tabs.vue";
import { ref } from "vue";

const selectedTab = ref(1);
const tabs = ref([
    { id: 1, name: "Multi Select", selected: true },
    { id: 2, name: "Grid View" },
]);
const handleTabChanged = (event) => {
    selectedTab.value = 0;
    setTimeout(() => {
        selectedTab.value = event.id;
    }, 500);
};
</script>
<style lang="scss" scoped>
.container {
    display: flex;
    flex-direction: column;
    margin: 40px;
}
:deep(.tabs-container) {
    display: flex;
    flex-direction: row;
    margin: 40px;
}
.v-enter-active,
.v-leave-active {
    transition: opacity 0.5s ease;
}

.v-enter-from,
.v-leave-to {
    opacity: 0;
}
</style>
