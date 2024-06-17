<template>
    <div
        class="modal-backdrop"
        tabindex="0"
        @keyup.esc="handleEscapePressed"
        @click.self="handleModalBackdropClicked"
    >
        <div @keydown.esc="handleEscapePressed" class="modal">
            <header v-if="showHeader" class="modal-header">
                <slot name="header">
                    {{ header }}
                    <button
                        type="button"
                        class="btn-close pointer"
                        aria-label="Close modal"
                        v-if="showCancel"
                        @click="handleCancelClicked"
                    >
                        x
                    </button>
                </slot>
            </header>
            <div class="modal-body">
                <slot name="body">{{ modalContent }}</slot>
            </div>
            <footer v-if="showFooter" class="modal-footer">
                <slot name="footer">
                    <button
                        class="pointer zaui-button cancel-button"
                        @click="handleCancelClicked"
                    >
                        {{ cancelButtonText }}
                    </button>
                    <button
                        class="pointer zaui-button ok-button"
                        @click="$emit('okClicked')"
                    >
                        {{ okButtonText }}
                    </button>
                </slot>
            </footer>
        </div>
    </div>
</template>

<script>
export default {
    name: "ModalDialog",
    props: {
        header: {
            type: String,
        },
        modalContent: {
            type: String,
        },
        showHeader: {
            type: Boolean,
            default: () => true,
        },
        showFooter: {
            type: Boolean,
            default: () => true,
        },
        showCancel: {
            type: Boolean,
            default: () => true,
        },
        cancelOnEscape: {
            type: Boolean,
            default: () => true,
        },
        cancelOnClickOutside: {
            type: Boolean,
            default: () => true,
        },
        allowCancel: {
            type: Boolean,
            default: () => true,
        },
        okButtonText: {
            type: String,
            default: () => "Ok",
        },
        cancelButtonText: {
            type: String,
            default: () => "Cancel",
        },
    },
    methods: {
        handleModalBackdropClicked(e) {
            if (this.cancelOnClickOutside) this.handleCancelClicked(e);
        },
        handleEscapePressed(e) {
            if (this.cancelOnEscape) this.handleCancelClicked(e);
        },
        handleCancelClicked(event) {
            if (this.allowCancel) {
                this.$emit("cancelClicked", event);
            }
        },
    },
};
</script>

<style lang="scss" scoped>
.modal-backdrop {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0, 0, 0, 0.3);
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 12;
}
.modal {
    background: #ffffff;
    box-shadow: 2px 2px 20px 1px;
    overflow-x: auto;
    display: flex;
    flex-direction: column;
    min-width: 400px;
    border-radius: 4px;
}
:root.dark .modal {
    background-color: rgba(0, 0, 0, 1);
    color: #ccc;
}
.modal-header,
.modal-footer {
    padding: 24px;
    display: flex;
}

.modal-header {
    position: relative;
    border-bottom: 1px solid #eeeeee;
    justify-content: space-between;
}

.modal-footer {
    border-top: 1px solid #eeeeee;
    justify-content: space-between;
    button {
        padding: 8px;
        width: 60px;
        background-color: #ccc;
        color: #ffffff;
        border-radius: 4px;
        cursor: pointer;
        border: none;
    }
}
.btn-close {
    position: absolute;
    top: 0;
    right: 0;
    border: none;
    font-size: 20px;
    padding: 16px;
    background: transparent;
}
.modal-body {
    position: relative;
    padding: 20px 24px;
}
</style>
