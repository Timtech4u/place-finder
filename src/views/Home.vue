<template>
  <div class="home">
    <Places msg="Find a Place"/>
  </div>
</template>

<script>
// @ is an alias to /src
import Places from "@/components/Places.vue";

export default {
    name: "home",
    components: {
        Places
    }
};
</script>
