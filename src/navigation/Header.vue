<template>
  <nav :class="{ dark: isDarkMode }">
    <button v-if="!isMenuActive" type="button" class="burger-button" title="Menu" @click="isMenuActive = !isMenuActive">
      <span class="burger-bar burger-bar--1"></span>
      <span class="burger-bar burger-bar--2"></span>
      <span class="burger-bar burger-bar--3"></span>
    </button>
  </nav>
  <nav :class="{ dark: isDarkMode }">
    <div class="nav-wrapper">
      <div class="nav-item">
        <router-link :to="{ name: 'Home' }">Home</router-link>
      </div>
      <div class="nav-item">
        <router-link :to="{ name: 'Components' }">Components</router-link>
      </div>
      <div class="nav-item right" @click="handleDarkModeClick">
        {{ isDarkMode ? "Light Mode" : "Dark Mode" }}
      </div>
    </div>
    <div v-if="isMenuActive" class="burger">
      <button v-if="isMenuActive" class="x-mark" @click="isMenuActive = !isMenuActive">X</button>
      <div class="nav-item" @click="isMenuActive = !isMenuActive">
        <router-link :to="{ name: 'Home' }">Home</router-link>
      </div>
      <div class="nav-item" @click="isMenuActive = !isMenuActive">
        <router-link :to="{ name: 'Components' }">Components</router-link>
      </div>
      <div class="nav-item">
        <span class="mode" @click="handleDarkModeClick">{{ isDarkMode ? "Light Mode" : "Dark Mode" }}</span>
      </div>
    </div>
  </nav>
</template>

<script setup>
import { ref } from "vue";
const isDarkMode = ref(true);
const isMenuActive = ref(false);
const handleDarkModeClick = () => {
  isDarkMode.value ? document.documentElement.classList.remove("dark") : document.documentElement.classList.add("dark");
  isDarkMode.value = !isDarkMode.value;
};
</script>

<style lang="scss" scoped>
.nav-wrapper {
  display: none;
  font-size: 24px;
  margin: 0px;
  padding: 24px 12px 12px 12px;
  border-bottom: solid 1px;
  display: none;
  @include breakpoint(medium) {
    display: flex;
  }
}
.burger {
  display: flex;
  flex-direction: column;
  font-size: 24px;
  margin: 0px;
  padding: 24px 12px 12px 36px;
  width: 80%;
  z-index: 4;
  position: absolute;
  top: 0px;
  background-color: #f5f5f5;
  height: 50%;
  box-shadow: 10px 5px 5px rgba(128, 128, 128, 0.08);
}

a,
.mode {
  text-decoration: none;
  color: inherit;
  &:hover {
    border-bottom: solid 1px;
  }
}
.nav-item {
  padding: 16px;
  cursor: pointer;
  &.right {
    margin-left: auto;
    margin-right: 16px;
  }
}

.burger-button,
.x-mark {
  position: relative;
  height: 30px;
  top: 0px;
  width: 40px;
  z-index: 99;
  border: 0;
  border-radius: 0;
  background-color: transparent;
  pointer-events: all;
  transition: transform 0.6s cubic-bezier(0.165, 0.84, 0.44, 1);
  cursor: pointer;
  margin: 20px 40px 0px 40px;
  @include breakpoint(medium) {
    display: none;
  }
}

.x-mark {
  font-size: 28px;
  color: #666;
  margin: 0px 8px 8px 4px;
  padding-top: 0px;
}

.burger-bar {
  position: absolute;
  top: 50%;
  right: 6px;
  left: 6px;
  height: 3px;
  width: auto;
  margin-top: -1px;
  transition: transform 0.6s cubic-bezier(0.165, 0.84, 0.44, 1), opacity 0.3s cubic-bezier(0.165, 0.84, 0.44, 1), background-color 0.6s cubic-bezier(0.165, 0.84, 0.44, 1);
  background-color: #666;
}

.dark {
  .x-mark {
    color: white;
  }
  .burger-bar {
    background-color: white;
  }
  .burger {
    background-color: #333;
    box-shadow: 10px 5px 5px rgba(0, 0, 0, 0.08);
  }
}
.burger-bar--1 {
  -webkit-transform: translateY(-6px);
  transform: translateY(-6px);
  top: 40%;
}

.burger-bar--2 {
  transform-origin: 100% 50%;
  transform: scaleX(1);
}

.burger-button:hover .burger-bar--2 {
  transform: scaleX(1);
}

.no-touchevents .burger-bar--2:hover {
  transform: scaleX(1);
}

.burger-bar--3 {
  transform: translateY(6px);
  top: 60%;
}

.burger.active .burger-button {
  transform: rotate(-180deg);
}

.burger.active .burger-bar {
  //   background-color: lighten($primary, 10);
}

.burger.active .burger-bar--1 {
  transform: rotate(45deg);
  top: 50%;
}

.burger.active .burger-bar--2 {
  opacity: 0;
}

.burger.active .burger-bar--3 {
  transform: rotate(-45deg);
  top: 50%;
}
</style>
