<template>
  <div>
    <h2 class="label">{{ label }}</h2>
    <div class="dropdown">
      <div
        @click="(isDropdown = !isDropdown), (isBg = !isBg)"
        class="dropdown-select"
        :class="{ 'dropdown-select_clicked': isDropdown }"
      >
        <span class="selected">{{ selectedValue.title }}</span>
        <div class="caret" :class="{ caret_rotate: isDropdown }"></div>
      </div>
      <ul class="dropdown-menu" :class="{ 'dropdown-menu_open': isDropdown }">
        <li
          @click="selectedItem(el)"
          class="dropdown-menu__item"
          :class="{ active: el.value === selectedValue.value }"
          v-for="el in pickerData"
          :key="el.value"
        >
          {{ el.title }}
        </li>
      </ul>
    </div>
    <!-- ekran ihtiyoriy joyi click bo'lganda dropdown yopilishi un kk -->
    <div
      v-if="isBg"
      @click.self="(isDropdown = false), (isBg = false)"
      class="bg"
    ></div>
  </div>
</template>

<script>
export default {
  props: {
    label: {
      type: String,
      default: () => "Label",
    },
    pickerData: {
      type: Array,
      default: () => [
        {
          title: "Select",
          value: 0,
        },
      ],
    },
  },
  data() {
    return {
      isDropdown: false,
      selectedValue: {
        title: "Select",
        value: 0,
      },
    };
  },
  methods: {
    selectedItem(evt) {
      this.isBg = false;
      this.isDropdown = false;
      this.selectedValue = { ...evt };
      this.$emit("get-date", evt);
    },
  },
};
</script>

<style lang="scss" scoped>
.label {
  color: #18181b;
  font-size: 0.8rem;
  font-weight: 600;
  margin-bottom: 8px;
}
.dropdown {
  min-width: 15em;
  position: relative;
  // dropdown-select
  &-select {
    background: #f0fdf4;
    color: #18181b;
    display: flex;
    align-items: center;
    justify-content: space-between;
    border: 1px solid #e4e4e7;
    border-radius: 0.5em;
    padding: 0.5em;
    cursor: pointer;
    transition: background 0.3s;
    // dropdown-select_clicked
    &_clicked {
      border: 1px solid #26989a;
      // box-shadow: 0 0 0.8em #26989a;
    }
    &:hover {
      // background: #323741;
      border-color: #26989a;
    }
    .caret {
      width: 0;
      height: 0;
      border-left: 5px solid transparent;
      border-right: 5px solid transparent;
      border-top: 6px solid #18181b;
      transition: 0.3s;
      // caret_rotate
      &_rotate {
        transform: rotate(180deg);
      }
    }
  }

  // dropdown-menu
  &-menu {
    list-style: none;
    padding: 0.2em 0.5em;
    background: #fff;
    border: 1px solid #e4e4e7;
    border-radius: 0.5em;
    box-shadow: 0 0.5em 1em rgba($color: #000000, $alpha: 0.2);
    color: #9fa5b5;
    position: absolute;
    top: 3em;
    left: 50%;
    width: 100%;
    transform: translateX(-50%);
    opacity: 0;
    display: none;
    transition: 0.2s;
    z-index: 2;
    max-height: 300px;
    overflow-y: auto;
    // menu_open
    &_open {
      display: block;
      opacity: 1;
    }
    // dropdown-menu__item
    &__item {
      padding: 0.7em 0.5em;
      margin: 0.3em 0;
      border-radius: 0.5em;
      cursor: pointer;
      &:hover {
        background: #e5e7eb;
      }
      &.active {
        background: #e5e7eb;
      }
    }
  }
}
.bg{
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: transparent;
  z-index: 1;
}
</style>
