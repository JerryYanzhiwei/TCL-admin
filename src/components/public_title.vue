<template>
  <p class="public_title">{{title}}</p>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      required: true,
      default: ''
    }
  }
}
</script>

<style lang="scss" scoped>
  .public_title {
    position: relative;
    font-size: 18px;
    font-weight: bold;
    border-bottom: 2px solid #F56C6C;
    max-width: 80px;
    // &::after {
    //   content: '';
    //   display: block;
    //   clear: both;
    //   position: absolute;
    //   top: 5px;
    //   left: 0;

    //   width: 3px;
    //   height: 15px;
    //   background-color: #dc1e32;
    // }
  }
</style>
