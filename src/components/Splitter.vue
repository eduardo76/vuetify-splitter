<template>

  <div class="splitter" v-resize="onResize" :class="{'cursor-resize': active}">
    <div ref="panel1" id="panel1" tile class="d-flex splitter-panels" :style="panel_left">
      <slot name="panel1" v-bind:left="left"></slot>
    </div>

    <div v-if="!noGutter" ref="splitter" @mousedown="spMouseDown" id="splitter" class="gutter" :class="{'active': active}" :style="style_gutter"></div>

    <div ref="panel2" id="panel2" tile class="d-flex splitter-panels" :style="panel_right">
      <slot name="panel2" v-bind:right="right"></slot>
    </div>
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

      noGutter: {
        type: Boolean,
        default: false
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
        this.gutterSize = parseInt(this.gutterSize);

        if (this.noGutter) {
          return {
            flexBasis: this.left + '%',
          }
        }

        return {
          flexBasis: 'calc(' + this.left + '% - '+ this.gutterSize +'px)',
        }
      },
      
      panel_right() {
        this.gutterSize = parseInt(this.gutterSize);

        if (this.noGutter) {
          return {
            flexBasis: this.right + '%',
          }
        }

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
  display: flex;
  flex-direction: row;
  height: 100%;
}

.splitter > .splitter-panels {
  flex: 0 1 auto;
  overflow-y: auto;
  min-height: 0px;
}

.splitter > .splitter-panels:hover {
  overflow-y: auto;
}

.splitter > .gutter {
  position: relative;
  cursor: e-resize;
  border: 1px solid #f8f8f8;
  background-color: #f8f8f8;
  flex-shrink: 0;
  flex-grow: 0;
  z-index: 1;
  transition: background-color 0.2s ease-in-out;
}

.splitter > .gutter.active  {
  background-color: #bebebe;
}
.splitter > .gutter:hover {
  background-color: #bebebe;
}

/* .splitter > .gutter::before {
  content: "";
  position: absolute;
  display: block;
  left: 0;
  width: 100%;
  top: 50%;
  height: 100px;
  margin-top: -100px;
  background-color: #bebebe;
  box-sizing: border-box;
  z-index: 1;
  border-radius: 10px;
} */

.cursor-resize {
  cursor: e-resize;
}

/* width */
::-webkit-scrollbar {
  width: 10px;
  height: 50px;
  opacity: 0;
}

/* Track */
::-webkit-scrollbar-track {
  background: #ececf0; 
}
 
/* Handle */
::-webkit-scrollbar-thumb {
  background: #d0d0d9; 
  border-radius: 10px;
}

/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
  background: #a9a9b1;
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