<template>
    <div class="list-view">
        <table class="list-view-table">
            <thead v-if="items.length">
                <tr>
                    <th
                        v-for="(header, index) in headers"
                        :key="index"
                        class="pointer"
                        :style="{ width: header.width || 'initial' }"
                        @click="sort(header.internalName)"
                        data-testid="header-column"
                    >
                        {{ header.label }}
                        <span
                            v-if="
                                header.internalName === sortColumn &&
                                sortDescending === true
                            "
                        >
                            <slot name="sort-icon-up"></slot>
                        </span>
                        <span
                            v-if="
                                header.internalName === sortColumn &&
                                sortDescending === false
                            "
                        >
                            <slot name="sort-icon-down"></slot>
                        </span>
                    </th>
                </tr>
            </thead>
            <tbody v-if="items.length">
                <tr
                    v-for="item in paginatedItems"
                    :key="item.id"
                    @click="$emit('itemClicked', item.id)"
                    data-testid="row"
                >
                    <td
                        v-for="(headerItem, tdIndex) in headers"
                        :key="`td-${tdIndex}`"
                        data-testid="column"
                    >
                        <!-- Pass in a external link for any column -->
                        <a
                            v-if="headerItem.linkURL"
                            target="_blank"
                            :href="
                                handleDynamicLinkURL(headerItem.linkURL, item)
                            "
                            @click.stop
                            class="column-link"
                            data-testid="linkURL"
                        >
                            {{ item[headerItem.internalName] }}
                        </a>
                        <!-- Pass in a router link for any column with dynamic params -->
                        <router-link
                            v-else-if="headerItem.routerLink"
                            :to="setRouterLink(headerItem.routerLink, item)"
                            class="column-link"
                            data-testid="router-link"
                        >
                            {{ item[headerItem.internalName] }}
                        </router-link>
                        <template v-else-if="headerItem.isHTML">
                            <span v-html="item[headerItem.internalName]">
                            </span>
                        </template>
                        <!-- Plain old column without a link -->
                        <template v-else>
                            <span> {{ item[headerItem.internalName] }}</span>
                        </template>
                    </td>

                    <td
                        v-if="showEdit"
                        @click="handleEditClicked(item.value)"
                        class="actions-cell"
                        data-testid="edit"
                    >
                        Edit
                    </td>
                </tr>
            </tbody>
            <tbody v-else>
                <tr v-if="showNoItemsMessage">
                    <td :colspan="headers.length">{{ noItemsMessage }}</td>
                </tr>
            </tbody>
        </table>
        <div
            v-if="items.length"
            class="p-3 lg:px-6 border-t dark:border-gray-800"
        ></div>
    </div>
    <list-view-pagination
        v-if="includePagination"
        :totalItems="items.length"
        :itemsPerPage="paginationItemsPerPage"
        :currentPage="currentPage"
        @pageChange="handlePageChange"
    />
    <modal-dialog
        v-if="showEditDialog"
        header="Edit Attraction"
        :modalContent="modalContent"
        @cancelClicked="showEditDialog = false"
        @okClicked="handleEditDialogOkClicked"
    />
</template>

<script>
import moment from "moment/dist/moment";
import ListViewPagination from "./ListViewPagination.vue";
import ModalDialog from "@/components/modal/ModalDialog.vue";

export default {
    // A Zaui built Vue 3 List View. Do not edit this code unless changing the structure.
    // You should consider this a blackbox component and only change if absolutely necessary.
    name: "GenericListView",
    components: { ListViewPagination, ModalDialog },
    emits: [
        "itemClicked",
        "sortChanged",
        "sortStarted",
        "paginationStarted",
        "paginationEnded",
    ],
    // components: {
    //     ListViewPagination,
    // },
    props: {
        /**
         * The headers to display in the table
         * @param {{string}} headers.label  - The label to be dislpayed
         * @param {{string}} headers.internalName - The internal name of the item. Used for sorting / routing
         * @param {{boolean}} headers.isDate - Used for sorting dates.
         * @param {{string}} headers.width - forcibly set the width of a column
         * @param {{RouterLink}} headers.routerLink - All the props of a Vue Router Link
         * @param {{string[]}} headers.routerLink.dynamicParams - Array of strings of internal names that can be mapped to dynamic route params
         * @param {{string}} headers.linkURL - The url of the the link to send. Can only use this or routerLink. You can pass 1 dynamic param wrapped in {{param}}. It must match a column. eg. {{host}} or https://{{themeName}}.zaui.net
         *
         */
        headers: {
            type: Array,
            required: true,
        },
        /**
         * An array of the items to show
         */
        items: {
            type: Array,
            required: true,
        },
        defaultSortColumn: {
            type: String,
        },
        defaultSortIsDescending: {
            type: Boolean,
        },
        showEdit: {
            type: Boolean,
            default: () => true,
        },
        disableSort: {
            type: Boolean,
            default: () => false,
        },
        noItemsMessage: {
            type: String,
            default: () => "No Items found",
        },
        showNoItemsMessage: {
            type: Boolean,
            default: false,
        },
        includePagination: {
            type: Boolean,
            default: true,
        },
        paginationItemsPerPage: {
            type: Number,
            default: 25,
        },
    },
    created() {
        this.sortColumn = this.defaultSortColumn ? this.defaultSortColumn : "";
        this.sortDescending = this.defaultSortIsDescending === true;
    },
    data() {
        return {
            sortColumn: "",
            sortDescending: false,
            currentPage: 1,
            showEditDialog: false,
            modalContent: "",
        };
    },
    watch: {
        includePagination(newval) {
            if (newval) {
                this.currentPage = 1;
            }
        },
        items(newval) {
            // If pagination is incldued
            if (this.includePagination) {
                this.currentPage = 1;
            }
        },
    },
    computed: {
        sortedItems() {
            // Return if there isnt a sort column selected
            if (!this.sortColumn) return this.items;
            const sortField = this.headers.find(
                (header) => header.internalName === this.sortColumn
            );
            if (!sortField) return this.items;
            return [...this.items].sort((a, b) => {
                if (sortField.isDate) {
                    return this.dateSort(a, b, sortField.dateFormat);
                } else {
                    // Otherwise do a regular sort
                    return this.alphabeticalSort(a, b);
                }
            });
        },
        dates() {
            return this.sortedItems.map((item) => item.startDate);
        },
        paginatedItems() {
            if (this.includePagination) {
                // Return sliced array based on pagination
                const startIndex =
                    (this.currentPage - 1) * this.paginationItemsPerPage;
                const endIndex = startIndex + this.paginationItemsPerPage;
                return this.sortedItems.slice(startIndex, endIndex);
            } else {
                return this.sortedItems;
            }
        },
    },
    methods: {
        formatDate(date) {
            return moment(date).format("YYYY-MM-DD");
        },
        // A function that allows Vue to emit an event prior to blocking the thread for heavy JS
        // computational logic, eg sorting. Subscribe to sortStarted to show a spinner
        async betterThanNextTick() {
            return new Promise((resolve) => {
                requestAnimationFrame(() => {
                    requestAnimationFrame(() => {
                        resolve();
                    });
                });
            });
        },
        async sort(sortBy) {
            this.$emit("sortStarted");
            await this.betterThanNextTick();
            if (!this.disableSort) {
                if (this.sortColumn === sortBy) {
                    this.sortDescending = !this.sortDescending;
                } else {
                    this.sortDescending = false;
                    this.sortColumn = sortBy;
                }
                this.$emit("sortChanged", {
                    sortColumn: this.sortColumn,
                    sortIsDescending: this.sortDescending,
                });
            }
        },
        dateFormatter(arg, dateFormat = "YYYY-MM-DD") {
            const newDate = moment(arg, dateFormat);
            if (!newDate.isValid()) {
                // If the date is null or invalid or screwed up return either infinity or negative infinity
                // to ensure it is always at the bottom or the top
                return this.sortDescending ? -Infinity : Infinity;
            }
            return parseInt(newDate.format("x"), 10);
        },
        dateSort(a, b, dateFormat) {
            if (this.sortDescending) {
                // Create the dates. The dateFormatter function handles removing nulls and invalidates
                const dateA = this.dateFormatter(
                    a[this.sortColumn],
                    dateFormat
                );
                const dateB = this.dateFormatter(
                    b[this.sortColumn],
                    dateFormat
                );
                return dateA < dateB ? 1 : dateA > dateB ? -1 : 0;
            } else {
                // Create the dates. The dateFormatter function handles removing nulls and invalidates
                const dateA = this.dateFormatter(
                    a[this.sortColumn],
                    dateFormat
                );
                const dateB = this.dateFormatter(
                    b[this.sortColumn],
                    dateFormat
                );
                return dateA < dateB ? -1 : dateA > dateB ? 1 : 0;
            }
        },
        alphabeticalSort(a, b) {
            // Get the values so its more readable further into the code block
            let sortA = a[this.sortColumn];
            let sortB = b[this.sortColumn];
            // Switch the order if its descending
            if (this.sortDescending) {
                [sortA, sortB] = [sortB, sortA];
            }
            // Make everything lowercase
            [sortA, sortB] = [sortA, sortB].map((s) =>
                (s || "").toString().toLocaleLowerCase()
            );
            // Return 0 if its the smae
            if (sortA !== sortB) {
                // Sort bu numbers if they are both numbers or use the new string compare to sort
                if (!isNaN(sortA) && !isNaN(sortB))
                    return Number(sortA) - Number(sortB);
                return stringCollator.compare(sortA, sortB);
            }
            return 0;
        },
        // Sets the router link details.
        setRouterLink(vueRouterLink, item) {
            if (vueRouterLink.dynamicParams) {
                const paramsFromDynamicParams =
                    vueRouterLink.dynamicParams.reduce((acc, param) => {
                        acc[param] = item[param];
                        return acc;
                    }, {});
                return { ...vueRouterLink, params: paramsFromDynamicParams };
            } else {
                return vueRouterLink;
            }
        },
        /**
         * Replaces a {{variable}} with its value from the item. Only handles 1
         * @param {String} link - the link. Can be just https://www.google.com or https://{{themeName.zaui.net}}
         * @param {Object} item - any object. If passing a variable wrapped in {{}} that variable must exist in here
         * @return {String} A URL that is used for a link
         */
        handleDynamicLinkURL(link, item) {
            // Just return the link immediately if there is no variable to replace
            if (link.includes("{{") && link.includes("}}")) {
                // Do it in a catch block so if it doesnt work it doesnt stop
                try {
                    // Need with curlys for replacement later
                    const withCurlys = link.substring(
                        link.indexOf("{"),
                        link.indexOf("}}") + 2
                    );
                    // Need no curlys to get the value from the item
                    const noCurlys = withCurlys.substring(
                        2,
                        withCurlys.length - 2
                    );
                    // Do the replacement here
                    return link.replace(withCurlys, item[noCurlys]);
                } catch {
                    return item;
                }
            } else {
                return item;
            }
        },
        async handlePageChange(newPage) {
            this.$emit("paginationStarted");
            await this.betterThanNextTick();
            this.currentPage = newPage;
            this.$emit("paginationEnded");
        },
        handleEditClicked(event) {
            this.showEditDialog = true;
            this.modalContent = `Really edit ${
                this.items.find((item) => item.value === event).label
            }?`;
        },
        handleEditDialogOkClicked() {
            this.showEditDialog = false;
        },
    },
};
const stringCollator = new Intl.Collator("en", {
    sensitivity: "accent",
    usage: "sort",
});
</script>

<style lang="scss" scoped>
a {
    text-decoration: none;
    color: #529bff;
    &:hover {
        text-decoration: underline;
    }
}
.list-view-table {
    width: 100%;
    border-spacing: 0;
    th {
        text-align: left;
        box-shadow: rgba(0, 0, 0, 0.12) 0px -1px 0px 0px inset;
        height: 48px;
        font-size: 14px;
        padding: 0px 16px;
    }
    tr {
        height: 48px;
    }
    td {
        border-bottom: thin solid rgba(0, 0, 0, 0.12);
        font-size: 14px;
        font-family: "roboto";
        padding: 0px 16px;
    }
}
.button-wrap {
    display: block;
}
.sort-icon {
    display: contents;
}
// styles for pagination component
.pagination-container {
    display: flex;
    justify-content: center;
    margin: 2rem auto;

    &:deep(button) {
        border: none;
        background: none;
        padding: 0 0.75rem;
        margin: 0 0.25rem;
        cursor: pointer;
        outline: none;
        transition: all 0.3s ease;
        border-radius: 4px;
        line-height: 2.5;

        &:hover:not([disabled]),
        &:focus:not([disabled]) {
            background-color: rgb(0, 0, 0, 0.05);
        }
        &:disabled {
            opacity: 0.75;
            cursor: not-allowed;
        }
        &.active {
            color: black;
            background-color: rgb(255, 255, 255, 0.05);
        }

        :root.dark &:hover:not([disabled]),
        :root.dark &:focus:not([disabled]) {
            background-color: rgb(255, 255, 255, 0.05);
        }
        &:disabled {
            opacity: 0.75;
            cursor: not-allowed;
        }
        :root.dark &.active {
            color: #ccc;
            background-color: rgb(255, 255, 255, 0.05);
        }
    }
    :root.dark &:deep(button) {
        color: #ccc;
    }
}
</style>
