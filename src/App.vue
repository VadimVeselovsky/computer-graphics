<template>
  <div id="app">
    <div style="margin-bottom: 20px">
      Задача:
      <select v-model="component">
        <option
          :value="option.value"
          v-for="option in components"
          :key="option.label"
        >
          {{ option.label }}
        </option>
      </select>
    </div>
    <component :is="component" />
  </div>
</template>

<script>
import Triangle from "@/components/Triangle";
import Ellipsis from "@/components/Ellipsis";

export default {
  data() {
    return { component: null };
  },
  computed: {
    component() {
      return Ellipsis;
    },

    components() {
      return [
        { value: Triangle, label: "№1, треугольник" },
        { value: Ellipsis, label: "№2, эллипс" },
      ];
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
