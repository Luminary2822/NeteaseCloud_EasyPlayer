<!--
 * @Description: 
 * @Author: Luminary
 * @Date: 2020-03-07 10:57:57
 * @LastEditTime: 2022-04-06 19:47:54
-->
<template>
  <div id="app">
    <top />
    <index />
  </div>
</template>

<script>
import top from './components/01.top.vue';
import index from './components/02.index.vue';
export default {
  name: 'app',
  components: {
    top,
    index
  }
};
</script>

<style >

</style>
