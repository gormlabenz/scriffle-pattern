<template>
  <div class="home">
    <Menu />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Menu from "@/components/Menu.vue"; // @ is an alias to /src

export default defineComponent({
  name: "Home",
  components: {
    Menu,
  },
});
</script>
