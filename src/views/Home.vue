<template>
  <div class="home">
    <BugList />
  </div>
</template>

<script>
import BugList from "../components/BugList.vue";

export default {
  name: "Home",
  components: { BugList },
};
</script>
