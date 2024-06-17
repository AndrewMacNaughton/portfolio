<template>
    <h2>Grid View</h2>
    <div></div>
    <div>
        <div class="props">
            <h3>Props</h3>
            <div class="grid">
                <div>
                    <input
                        name="paginationItemsPerPage"
                        class="input-class skinny"
                        type="number"
                        v-model="paginationItemsPerPage"
                    />
                    <div>paginationItemsPerPage</div>
                    <div>Controls the number of items per page</div>
                </div>
                <div>
                    <input
                        name="showEdit"
                        class="input-class skinny"
                        type="checkbox"
                        v-model="showEditButton"
                    />
                    <div>showEditButton</div>
                    <div>Show the edit button on the end</div>
                </div>
                <div>
                    <multi-select-input
                        :selectOptions="headersForSort"
                        @itemAdded="handleItemAdded"
                        @itemRemoved="handleItemRemoved"
                        placeHolderText="Custom Headers"
                    />
                    headers
                    <div>Choose which headers to show</div>
                </div>
            </div>
        </div>
        <div class="control-wrapper">
            <h3>Live Control</h3>
            <grid-view
                :headers="
                    filteredHeadersToShow.length > 0
                        ? filteredHeadersToShow
                        : headers
                "
                :items="attractions"
                :paginationItemsPerPage="paginationItemsPerPage"
                :showEdit="showEditButton"
                defaultSortColumn="label"
            >
                <template #sort-icon-up><i class="arrow up"></i></template>
                <template #sort-icon-down><i class="arrow down"></i></template>
            </grid-view>
        </div>
    </div>
</template>

<script setup>
import { ref, computed } from "vue";
const paginationItemsPerPage = ref(10);
const showEditButton = ref(true);
const headersToShow = ref([]);
import GridView from "./GridView.vue";
import MultiSelectInput from "../multi/MultiSelectInput.vue";
const handleItemAdded = (e) => {
    headersToShow.value.push(e.item.label);
};
const handleItemRemoved = (e) => {
    const index = headersToShow.value.findIndex(
        (item) => item === e.item.label
    );
    headersToShow.value.splice(index, 1);
};

const headers = [
    { label: "id", internalName: "value", width: "60px" },
    { label: "Name", internalName: "label", width: "240px" },

    { label: "Country", internalName: "country", width: "140px" },
    {
        label: "Built/Discovered",
        internalName: "discovered",
        width: "240px",
        isDate: true,
    },
    {
        label: "More Info",
        internalName: "info",
        width: "240px",
        linkURL: "https://google.com/search?q={{label}}",
    },
];
const filteredHeadersToShow = computed(() => {
    return headers.filter((item) => headersToShow.value.includes(item.label));
});

const headersForSort = headers.map((item, index) => {
    return {
        value: index,
        label: item.label,
        internalName: item.internalName,
    };
});
const attractions = [
    {
        value: 1,
        label: "Eiffel Tower",
        country: "France",
        discovered: "1652-11-10",
    },
    {
        value: 2,
        label: "The Louvre",
        country: "France",
        discovered: "1620-01-26",
    },
    {
        value: 3,
        label: "Statue of Liberty",
        country: "USA",
        discovered: "1607-01-20",
    },
    {
        value: 4,
        label: "Machu Picchu",
        country: "Peru",
        discovered: "1740-08-28",
    },
    {
        value: 5,
        label: "Great Wall of China",
        country: "China",
        discovered: "1651-08-25",
    },
    {
        value: 6,
        label: "Taj Mahal",
        country: "India",
        discovered: "1742-08-29",
    },
    {
        value: 7,
        label: "Pyramids of Giza",
        country: "Egypt",
        discovered: "1720-10-26",
    },
    {
        value: 8,
        label: "Colosseum",
        country: "Italy",
        discovered: "1801-09-23",
    },
    {
        value: 9,
        label: "Sagrada Família",
        country: "Spain",
        discovered: "1741-11-14",
    },
    {
        value: 10,
        label: "Niagara Falls",
        country: "Canada",
        discovered: "1856-07-25",
    },
    {
        value: 11,
        label: "Grand Canyon",
        country: "USA",
        discovered: "1863-06-28",
    },
    { value: 12, label: "Petra", country: "", discovered: "1803-12-13" },
    {
        value: 13,
        label: "Angkor Wat",
        country: "Cambodia",
        discovered: "1866-01-07",
    },
    {
        value: 14,
        label: "Museum of Modern Art (MoMA)",
        country: "USA",
        discovered: "1758-01-04",
    },
    { value: 15, label: "Burj Khalifa", country: "", discovered: "1752-09-29" },
    {
        value: 16,
        label: "Sydney Opera House",
        country: "Australia",
        discovered: "1716-11-03",
    },
    {
        value: 17,
        label: "Golden Gate Bridge",
        country: "USA",
        discovered: "1900-06-05",
    },
    {
        value: 18,
        label: "Mount Everest",
        country: "Nepal",
        discovered: "1911-01-11",
    },
    {
        value: 19,
        label: "Venice Canals",
        country: "Italy",
        discovered: "1926-10-20",
    },
    {
        value: 20,
        label: "Christ the Redeemer",
        country: "Brazil",
        discovered: "1936-01-20",
    },
    {
        value: 21,
        label: "Acropolis of Athens",
        country: "Greece",
        discovered: "1982-11-22",
    },
    {
        value: 22,
        label: "Forbidden City",
        country: "",
        discovered: "1984-03-02",
    },
    {
        value: 23,
        label: "Buckingham Palace",
        country: "England",
        discovered: "1988-08-31",
    },
    {
        value: 24,
        label: "St. Peter's Basilica",
        country: "",
        discovered: "1996-10-24",
    },
    {
        value: 25,
        label: "Museum of Islamic Art",
        country: "",
        discovered: "2014-07-10",
    },
].map((item) => {
    return {
        ...item,
        info: "More Info",
    };
});
</script>

<style lang="scss" scoped>
.grid {
    display: grid;
    @include breakpoint(medium) {
        grid-template-columns: repeat(3, 1fr);
    }
    gap: 16px;
    div {
        padding: 4px 0px;
    }
}
.prop-wrapper {
    display: flex;
    flex-direction: column;
    gap: 2em;
}
.prop {
    display: flex;
    flex-direction: column;
    gap: 4px;
}

.input-wrapper {
    @include breakpoint(medium) {
        margin: 0px 20px;
    }
}
.arrow {
    border: solid #529bff;
    border-width: 0 2px 2px 0;
    display: inline-block;
    padding: 3px;
    position: relative;
    margin-left: 3px;
}
.up {
    transform: rotate(-135deg);
    -webkit-transform: rotate(-135deg);
    top: 2px;
}

.down {
    transform: rotate(45deg);
    -webkit-transform: rotate(45deg);
    top: -2px;
}
:deep input[type="text"] {
    height: 40px;
    font-size: 16px;
    line-height: 24px;
    border: solid 1px #ccc;
    border-radius: 4px;
    color: #5f5f5f !important;
    padding-left: 12px;
    &:focus-visible {
        outline: none;
    }
    &.skinny {
        width: 40px;
    }
}
:deep input[type="text"] {
    height: 50px;
}
:deep
    dropdown:not(:focus-within)
    .selected-label.placeholder-label.placeholder-label {
    color: #ddd;
}
:deep .suggestion-list {
    max-height: 400px;
    width: 449px;
    overflow-y: scroll;
    border-top-left-radius: 0px;
    border-top-right-radius: 0px;
}
:deep input.expanded {
    border-bottom-left-radius: 0px;
    border-bottom-right-radius: 0px;
    border-bottom: none;
    padding-top: 8px;
}
:deep thead,
:deep .actions-cell {
    cursor: pointer;
}
</style>
