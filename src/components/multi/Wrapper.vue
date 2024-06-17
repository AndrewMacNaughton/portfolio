<template>
    <h2>Multi Select Input</h2>
    <div>
        An auto complete, multi select control. You can update the props below
        in real time and see the control in action
    </div>
    <div class="grid">
        <div class="input-wrapper">
            <h3>Live Control</h3>
            <multi-select-input
                :selectOptions="attractions"
                :numberOfItemsToShowProp="numberOfItemsToShow"
                :maxSelectionsAllowed="numberOfSelections"
                :showSelectAll="isShowSelectAll"
                placeHolderText="Please Select Your Favorite Attractions"
            />
        </div>
        <div class="props">
            <h3>Configuration Props:</h3>
            <div class="prop-wrapper">
                <div class="prop">
                    <div>
                        <input
                            name="numberOfItemsToShow"
                            class="input-class skinny"
                            type="number"
                            v-model="numberOfItemsToShow"
                        />
                        numberOfItemsToShow
                    </div>
                    <div>
                        <label for="numberOfItemsToShow"
                            >Number of selected items to show before adding & X
                            more</label
                        >
                    </div>
                </div>
                <div class="prop">
                    <div>
                        <input
                            name="numberOfItemsToShow"
                            class="input-class skinny"
                            type="number"
                            v-model="numberOfSelections"
                        />
                        numberOfSelections
                    </div>
                    <div>
                        <label for="numberOfItemsToShow"
                            >Maximum number of selected items
                        </label>
                    </div>
                </div>
                <div class="prop">
                    <input
                        name="isShowSelectAll"
                        class="input-class skinny"
                        type="checkbox"
                        v-model="isShowSelectAll"
                    />
                    <div>
                        <label for="isShowSelectAll"
                            >Show Select All at top of Select
                        </label>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import MultiSelectInput from "./MultiSelectInput.vue";
import { ref } from "vue";
const numberOfItemsToShow = ref(1);
const numberOfSelections = ref(3);
const isShowSelectAll = ref(true);
const attractions = [
    { value: 1, label: "Eiffel Tower" },
    { value: 2, label: "The Louvre" },
    { value: 3, label: "Statue of Liberty" },
    { value: 4, label: "Machu Picchu" },
    { value: 5, label: "Great Wall of China" },
    { value: 6, label: "Taj Mahal" },
    { value: 7, label: "Pyramids of Giza" },
    { value: 8, label: "Colosseum" },
    { value: 9, label: "Sagrada Família" },
    { value: 10, label: "Niagara Falls" },
    { value: 11, label: "Grand Canyon" },
    { value: 12, label: "Petra" },
    { value: 13, label: "Angkor Wat" },
    { value: 14, label: "Museum of Modern Art (MoMA)" },
    { value: 15, label: "Burj Khalifa" },
    { value: 16, label: "Sydney Opera House" },
    { value: 17, label: "Golden Gate Bridge" },
    { value: 18, label: "Mount Everest" },
    { value: 19, label: "Venice Canals" },
    { value: 20, label: "Christ the Redeemer" },
    { value: 21, label: "Acropolis of Athens" },
    { value: 22, label: "Forbidden City" },
    { value: 23, label: "Buckingham Palace" },
    { value: 24, label: "St. Peter's Basilica" },
    { value: 25, label: "Museum of Islamic Art" },
];
</script>

<style lang="scss" scoped>
.grid {
    @include breakpoint(medium) {
        display: grid;
        grid-template-columns: 40% 60%;
        .props {
            grid-column-start: 1;
            grid-row-start: 1;
        }
        .input-wrapper {
            grid-column-start: 2;
            grid-row-start: 1;
        }
    }
    @include breakpoint(large) {
        display: grid;
        grid-template-columns: 50% 50%;
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
.input-class,
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
.input-wrapper {
    width: 450px;
    @include breakpoint(medium) {
        margin: 0px 20px;
    }
}
</style>
