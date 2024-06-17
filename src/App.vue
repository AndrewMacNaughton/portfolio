<script setup>
import TheHeader from "@/navigation/Header.vue";
</script>

<template>
    <div class="container">
        <the-header></the-header>
        <router-view v-slot="{ Component, route }">
            <Transition mode="out-in">
                <div :key="route.name">
                    <component :is="Component" />
                </div>
            </Transition>
        </router-view>
    </div>
</template>

<style lang="scss">
@font-face {
    font-family: roboto;
    src: url("./assets/RobotoMono-VariableFont_wght.ttf");
}
html {
    margin: 0px;
    padding: 0px;
}
:root {
    color: #666;
    background-color: #f5f5f5;
}
:root.dark {
    background-color: #333;
    color: #ccc;
}
body {
    font-family: "roboto";
    margin: 0px;
    .container {
        margin: 0px;
    }
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
