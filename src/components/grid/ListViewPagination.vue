<template>
    <div class="pagination-container">
        <slot name="previousButton">
            <button @click="prevPage" :disabled="currentPage == 1">
                {{ prevButtonText }}
            </button>
        </slot>
        <button
            v-for="page in pages"
            :key="page"
            @click="goToPage(page)"
            :class="{ active: currentPage === page }"
        >
            {{ page }}
        </button>
        <slot class="nextButton">
            <button @click="nextPage" :disabled="currentPage == pageCount">
                {{ nextButtonText }}
            </button>
        </slot>
    </div>
</template>

<script>
export default {
    name: "ListViewPagination",
    props: {
        totalItems: {
            type: Number,
            required: true,
        },
        itemsPerPage: {
            type: Number,
            default: 10,
        },
        currentPage: {
            type: Number,
            default: 1,
        },
        maxPagesToShow: {
            type: Number,
            default: 5,
        },
        prevButtonText: {
            type: String,
            default: "Prev",
        },
        nextButtonText: {
            type: String,
            default: "Next",
        },
    },
    computed: {
        pageCount() {
            // Calculate number of pages required
            return Math.ceil(this.totalItems / this.itemsPerPage);
        },
        pages() {
            // Set the start page number (digit)
            const startPage = Math.max(
                1,
                this.currentPage - Math.floor(this.maxPagesToShow / 2)
            );
            // Set the end page number (digit)
            const endPage = Math.min(
                this.pageCount,
                startPage + this.maxPagesToShow - 1
            );
            // Generate an array starting from 1 and max to endPage
            return Array.from(
                { length: endPage - startPage + 1 },
                (_, i) => startPage + i
            );
        },
    },
    methods: {
        // Emit event when user clicks on Prev button
        prevPage() {
            if (this.currentPage > 1) {
                this.$emit("pageChange", this.currentPage - 1);
            }
        },
        // Emit event when user clicks on Next Button
        nextPage() {
            if (this.currentPage < this.pageCount) {
                this.$emit("pageChange", this.currentPage + 1);
            }
        },
        // Emit event when user clicks on digits
        goToPage(page) {
            if (this.currentPage !== page) {
                this.$emit("pageChange", page);
            }
        },
    },
};
</script>
