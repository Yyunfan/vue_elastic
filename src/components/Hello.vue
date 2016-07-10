<template>
  <draggable-header-view>
    <template slot="content">
      <p>上拉试试⬆️</p>
      <p>微博上看到的下图@你丫才美工</p>
      <img src="../assets/haha.gif" width="100%" />
    </template>
  </draggable-header-view>
</template>

<script>
  import draggableHeaderView from './draggableHeaderView'
  export default {
    data () {
      return {

      }
    },
    components: {
      draggableHeaderView
    }
  }
</script>
