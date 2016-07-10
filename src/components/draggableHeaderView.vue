<template>
  <div class="draggable-header-view"
    @mousedown="startDrag" @touchstart="startDrag"
    @mousemove="onDrag" @touchmove="onDrag"
    @mouseup="stopDrag" @touchend="stopDrag" @mouseleave="stopDrag">
    <div class="content smx" id="newAm">
      <img src="../assets/mn.jpg" style="display:block;width:100%;"/>
      <span class="down-am"></span>
    </div>
    <svg class="bg" width="320" height="560">
      <path :d="headerPath" fill="#2d2b38"></path>
    </svg>
    <div class="content smx">
      <slot name="content"></slot>
    </div>
  </div>
</template>
<script>
  import dynamics from 'dynamics.js'
  export default {
    name: 'draggableHeaderView',
    data () {
      return {
        dragging: false,
        // quadratic bezier control point
        c: { x: 160, y: 150 },
        // record drag start point
        start: { x: 0, y: 0 }
      }
    },
    computed: {
      headerPath: function () {
        return 'M0,227 L320,227 320,0' +
          'Q' + this.c.x + ',' + this.c.y +
          ' 0,0'
      },
      contentPosition: function () {
        var dy = this.c.y - 160
        var dampen = dy > 0 ? 2 : 4
        return {
          transform: 'translate3d(0,' + dy / dampen + 'px,0)'
        }
      }
    },
    methods: {
      startDrag: function (e) {
        e = e.changedTouches ? e.changedTouches[0] : e
        this.dragging = true
        this.start.x = e.pageX
        this.start.y = e.pageY
      },
      onDrag: function (e) {
        e = e.changedTouches ? e.changedTouches[0] : e
        if (this.dragging) {
          this.c.x = 160 + (e.pageX - this.start.x)
          // dampen vertical drag by a factor
          var dy = e.pageY - this.start.y
          var dampen = dy > 0 ? 1.5 : 4
          this.c.y = 160 + dy / dampen
        }
      },
      stopDrag: function () {
        if (this.dragging) {
          this.dragging = false
          dynamics.animate(this.c, {
            x: 160,
            y: 150
          }, {
            type: dynamics.spring,
            duration: 700,
            friction: 280
          })
        }
      }
    }
  }
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
  h1 {
    font-weight: 300;
    font-size: 1.8em;
    margin-top: 0;
  }
  a {
    color: #fff;
  }
  .draggable-header-view {
    background-color: #fff;
    box-shadow: 0 4px 16px rgba(0,0,0,.15);
    width: 320px;
    height: 560px;
    overflow: hidden;
    margin: 30px auto;
    position: relative;
    font-family: 'Roboto', Helvetica, Arial, sans-serif;
    color: #fff;
    font-size: 14px;
    font-weight: 300;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
  }
  .draggable-header-view .bg {
    position: absolute;
    top: 0;
    left: 0;
    z-index: 10;
  }
  .draggable-header-view .header, .draggable-header-view .content {
    position: relative;
    z-index: 1;
    padding: 30px;
    box-sizing: border-box;
  }
  .draggable-header-view .header {
    height: 160px;
  }
  .draggable-header-view .content {
    color: #333;
    line-height: 1.5em;
  }
  .smx {
    padding: 0 !important;
  }
  .down-am {
    position: absolute;
    width: 60px;
    height: 71px;
    background: url(../assets/down.png) no-repeat center;
    background-size: contain;
    left: 10px;
    bottom: -60px;
    z-index: 999;
    color: #813bb6;
  }
</style>
