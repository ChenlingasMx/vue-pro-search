<template>
  <div>
    <div class="home">
      <img alt="Vue logo" src="../assets/logo.png" />
    </div>
  </div>
</template>

<script>
import str from '../../../README.md';

export default {
  name: 'Home',
  data() {
    return {
      markdown: str,
    };
  },
  components: {},
};
</script>

<style scoped>
.markdown {
  text-align: left;
  max-width: 630px;
  padding-bottom: 120px;
}
</style>
