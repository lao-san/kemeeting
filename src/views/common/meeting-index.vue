<template>
  <div>{{id}}</div>
</template>
<script>
export default {
  name: "",
  data() {
    return {
      id: ""
    };
  },
  components: {},
  computed: {},
  beforeMount() {},
  activated() {
    this.getId();
  },
  methods: {
    getId() {
      this.id = this.$route.params.id;
    }
  },
  watch: {}
};
</script>
<style scoped>
</style>
