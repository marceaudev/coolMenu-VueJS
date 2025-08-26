<script setup>
import { ref, onBeforeUnmount } from 'vue'

const openMenu = ref(false)
const isNavHovered = ref(false)
const isBtnHovered = ref(false)
let closeTimeout = null

const startCloseTimer = () => {
  clearTimeout(closeTimeout)
  closeTimeout = setTimeout(() => {
    openMenu.value = false
  }, 1000)
}

const onBtnEnter = () => {
  isBtnHovered.value = true
  openMenu.value = true
  clearTimeout(closeTimeout)
}

const onBtnLeave = () => {
  isBtnHovered.value = false
  if (!isNavHovered.value) startCloseTimer()
}

const keepMenuOpen = () => {
  clearTimeout(closeTimeout)
  openMenu.value = true
  isNavHovered.value = true
}

const closeMenu = () => {
  isNavHovered.value = false
  if (!isBtnHovered.value) startCloseTimer()
}

onBeforeUnmount(() => {
  clearTimeout(closeTimeout)
})
</script>

<template>
  <header>
    <div
      class="menuBtn"
      @mouseenter="onBtnEnter"
      @mouseleave="onBtnLeave"
      :class="{ activeBtn: isNavHovered }"
    >
      <font-awesome-icon icon="fa-solid fa-bars" />
    </div>
    <Transition name="fade-slide" appear>
      <div class="menu" v-show="openMenu">
        <nav @mouseenter="keepMenuOpen" @mouseleave="closeMenu">
          <a href="">Home</a>
          <a href="">Works</a>
          <a href="">Projects</a>
          <a href="">FAQ</a>
          <a href="">Login</a>
        </nav>
      </div>
    </Transition>
  </header>
</template>

<style scoped>
header {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  position: relative;
}

.menuBtn {
  width: 40px;
  height: 40px;
  border-radius: 100%;
  background-color: var(--main-white);
  display: flex;
  align-items: center;
  justify-content: center;
  transition: transform 1.5s ease;
}

.menuBtn:hover {
  cursor: pointer;
  transform: rotate(360deg) scale(1.05);
  transition: transform 0.2s ease;
}

.menuBtn.activeBtn {
  transform: rotate(360deg) scale(1.05);
  transition: none;
}

.menuBtn svg {
  color: var(--main-black);
  font-size: 18px;
}

.menu {
  position: absolute;
  bottom: -40px;
}

.showMenu {
  display: block;
}

.menu nav {
  display: flex;
  align-items: center;
  gap: 15px;
}

.menu nav a {
  text-decoration: none;
  color: var(--main-black);
  background-color: var(--main-white);
  padding: 5px 10px;
  border-radius: 10px;
  font-size: 18px;
  transition: transform 0.2s ease-in-out;
}

.menu nav a:hover {
  transform: scale(1.05);
}

/* --- Animation Vue <Transition name="fade-slide"> --- */

.fade-slide-enter-from,
.fade-slide-leave-to {
  opacity: 0;
  transform: translateY(-6px) scale(0.98);
  pointer-events: none;
}

.fade-slide-enter-to,
.fade-slide-leave-from {
  opacity: 1;
  transform: translateY(0) scale(1);
}

.fade-slide-enter-active,
.fade-slide-leave-active {
  transition:
    opacity 180ms ease,
    transform 180ms ease;
  will-change: opacity, transform;
}
</style>
