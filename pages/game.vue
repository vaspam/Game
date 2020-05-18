<template>
  <section>
    <hand/>
    <sidebar/>
  </section>
</template>

<script>
import hand from "~/components/hand.vue";
import sidebar from "~/components/sidebar.vue";

export default {
  data() {
    return {};
  },
  components: {
    hand,
    sidebar
  },
  computed: {},
  methods: {}
};
</script>

<style scoped>
</style>
