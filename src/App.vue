<template>
  <div id="app" class="gw">
    <HelloWorld/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld';

export default {
  name: 'App',
  components: {
    HelloWorld,
  },
};
</script>

<style lang="less" scoped>
#app {
  font-family: -apple-system,
              BlinkMacSystemFont,
              "Segoe UI",
              Helvetica,
              Arial,
              sans-serif,
              "Apple Color Emoji",
              "Segoe UI Emoji",
              "Segoe UI Symbol";
}

.gw {
  max-width: 350px;
  min-height: 300px;
  background: #fff;
  border: 1px solid #ddd;
  cursor: default;
}
</style>
