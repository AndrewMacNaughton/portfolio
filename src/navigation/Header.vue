<template>
    <nav>
        <div class="nav-wrapper">
            <div class="nav-item">
                <router-link :to="{ name: 'Home' }">Home</router-link>
            </div>
            <div class="nav-item">
                <router-link :to="{ name: 'Components' }"
                    >Components</router-link
                >
            </div>
            <div class="nav-item"> <router-link :to="{ name: 'About' }"
                    >About</router-link
                ></div>
            <div class="nav-item right" @click="handleDarkModeClick">
                {{ isDarkMode ? "Light Mode" : "Dark Mode" }}
            </div>
        </div>
    </nav>
</template>

<script setup>
import { ref } from "vue";
const isDarkMode = ref(true);
const handleDarkModeClick = () => {
    isDarkMode.value
        ? document.documentElement.classList.remove("dark")
        : document.documentElement.classList.add("dark");
    isDarkMode.value = !isDarkMode.value;
};
</script>

<style lang="scss" scoped>
.nav-wrapper {
    display: flex;
    font-size: 24px;
    margin: 0px;
    padding: 24px 12px 12px 12px;
    border-bottom: solid 1px;
    .nav-item {
        padding: 16px;
        cursor: pointer;
        a {
            text-decoration: none;
            color: inherit;
            &:hover {
                border-bottom: solid 1px;
            }
        }
        &.right {
            margin-left: auto;
            margin-right: 16px;
        }
    }
}
</style>
