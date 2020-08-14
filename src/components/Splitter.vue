<template>

  <div class="splitter h-100" v-resize="onResize" :class="{'cursor-resize': active}">
    <v-sheet ref="panel1" id="panel1" tile class="d-flex split-block" :style="panel_left">
      <slot name="panel1" v-bind:left="left"></slot>
    </v-sheet>

    <div ref="splitter" @mousedown="spMouseDown" id="splitter" class="gutter" :class="{'active': active}" :style="style_gutter"></div>

    <v-sheet ref="panel2" id="panel2" tile class="d-flex split-block" :style="panel_right">
      <slot name="panel2" v-bind:right="right"></slot>
    </v-sheet>
  </div>

</template>

<script>
  export default {
    name: 'splitter',

    props: {
      value: {
        type: [Number, String],
        default: 300
      },

      gutterSize: {
        type: [Number, String],
        default: 10
      },

      minWidth: {
        type: [Number, String],
        default: 300
      }
    },

    data() {
      return {
        splitter: null,
        panel1: null,
        panel2: null,
        window_width: null,
        percentage: null,
        left: null,
        right: null,
        active: false,
      }
    },

    mounted () {
      this.init();
    },

    methods: {

      init() {
        
        this.splitter = this.$refs.splitter;
        this.panel1   = this.$refs.panel1;
        this.panel2   = this.$refs.panel2;

        // this.splitter.addEventListener("mousedown", this.spMouseDown);

        this.setPosition(this.value);

      },

      spMouseDown(event) {
        // this.splitter.removeEventListener("mousedown", this.spMouseDown);
        window.addEventListener("mousemove", this.spMouseMove);
        window.addEventListener("mouseup", this.spMouseUp);

        document.body.classList.add('noselect');
        this.active = true;
      },

      spMouseUp(event) {
        window.removeEventListener("mousemove", this.spMouseMove);
        window.removeEventListener("mouseup", this.spMouseUp);
        this.splitter.addEventListener("mousedown", this.spMouseDown);
        
        document.body.classList.remove('noselect')
        this.active = false;
      },

      spMouseMove(event) {
        if (event.clientX > this.minWidth) {
          this.setPosition(event.clientX);
          this.$emit('input', event.clientX);
        }
      },

      setPosition(position) {
        this.percentage = parseFloat(((position/this.window_width) * 100).toFixed(4));

        this.left  = this.percentage;
        this.right = 100 - this.left;
      },

      onResize() {
        this.window_width = window.innerWidth;
      }
    },

    computed: {
      panel_left() {
        return {
          flexBasis: 'calc(' + this.left + '% - '+ this.gutterSize +'px)',
          overflowY: 'auto',
          height: "410px"
        }
      },
      panel_right() {
        return {
          flexBasis: 'calc(' + this.right + '% - '+ this.gutterSize +'px)'
        }
      },
      style_gutter() {
        return {
          width: this.gutterSize +'px'
        }
      }
    },

    watch: {
      value(newValue, oldValue) {
        this.setPosition(newValue);
      }
    },

  }
</script>

<style scoped>

.splitter {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -ms-flex-wrap: nowrap;
  flex-wrap: nowrap;
  -webkit-box-orient: horizontal;
  -webkit-box-direction: normal;
  -ms-flex-direction: row;
  flex-direction: row;
  -webkit-box-pack: justify;
  -ms-flex-pack: justify;
  justify-content: space-between;
}

.h-100 {
    height: 100% !important;
}

.splitter > .split-block {
  position: relc;
  -webkit-box-flex: 1;
  -ms-flex-positive: 1;
  flex-grow: 1;
  -ms-flex-preferred-size: 0;
  flex-basis: 0;
  overflow: hidden;
}

.splitter > .gutter {
  -ms-flex-negative: 0;
  flex-shrink: 0;
  -webkit-box-flex: 0;
  -ms-flex-positive: 0;
  flex-grow: 0;
  border: 1px solid #f8f8f8;
  background-color: #f8f8f8;
  cursor: e-resize;
  z-index: 1;
  position: relative;
}

.splitter > .gutter.active  {
  background-color: #bebebe;
}

.splitter > .gutter::before {
  content: "";
  z-index: 1;
  display: block;
  position: absolute;
  left: 0;
  width: 100%;
  top: 50%;
  height: 24px;
  margin-top: -100px;
  background-color: #bebebe;
  box-sizing: border-box;
}

.cursor-resize {
  cursor: e-resize;
}

</style>

<style>
.noselect {
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}
</style>