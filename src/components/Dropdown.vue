<template>
  <div class="dropdown">
    <button
      type="button"
      class="dropdown-toggle"
      :class="{ active: isActive }"
      @click.stop="toggleActive"
    >
      Редактировать таблицу
    </button>
    <div v-if="isActive" class="dropdown-menu">
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isActive: false,
    };
  },
  methods: {
    handlerOutsideClick(e) {
      if (!e.target.closest('.dropdown')) this.toggleActive();
    },
    toggleActive() {
      this.isActive = !this.isActive;
    },
  },
  watch: {
    isActive(value) {
      if (value) document.addEventListener('click', this.handlerOutsideClick);
      else document.removeEventListener('click', this.handlerOutsideClick);
    },
  },
};
</script>

<style lang="scss" scoped>
.dropdown {
  min-width: 300px;
  display: inline-block;
  position: relative;
  z-index: 100;
  cursor: pointer;
}
.dropdown-toggle {
  width: 100%;
  height: 100%;
  text-align: center;
  outline: none;
}
.dropdown-menu {
  position: absolute;
  top: 100%;
  left: 0;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: stretch;
  border: 1px solid #000;
}
.dropdown-item {
  background: #fff;
  text-align: left;
  padding: 5px;
  &:not(:last-child) {
    border-bottom: 1px solid #000;
  }
}
.state-icon {
  display: inline-block;
  vertical-align: bottom;
  width: 1.5em;
  height: 1.5em;
  position: relative;
  &::after,
  &::before {
    position: absolute;
    content: '';
    width: 2px;
    height: 100%;
    background-color: #757575;
  }
}
.icon-checked {
  &::before {
    left: 0;
    top: 50%;
    height: 50%;
    transform: rotate(-45deg);
    transform-origin: top;
  }
  &::after {
    top: 4px;
    left: 100%;
    height: calc(100% - 1px);
    transform: rotate(45deg);
    transform-origin: top;
  }
}
.icon-close {
  &::before {
    left: 50%;
    transform: rotate(-45deg);
  }
  &::after {
    left: 50%;
    transform: rotate(45deg);
  }
}
.active {
  background-color: rgb(217, 217, 217);
}
</style>