<template>
  <div class="grid-content">
    <grid-layout
      :layout.sync="layout"
      :col-num="colNum"
      :max-rows="maxRows"
      :row-height="20"
      :margin="[0, 0]"
      :is-draggable="draggable"
      :is-resizable="resizable"
      :responsive="false"
      :vertical-compact="false"
      :prevent-collision="true"
      :use-css-transforms="true"
      @layout-updated="layoutUpdatedEvent"
    >
      <!-- @layout-created="layoutCreatedEvent"
      @breakpoint-changed="breakpointChangedEvent"

      @layout-before-mount="layoutBeforeMountEvent"
      @layout-mounted="layoutMountedEvent"
      @layout-ready="layoutReadyEvent" -->
      <grid-item
        v-for="(item, inx) in layout"
        :key="inx"
        :static="item.static"
        :x="item.x"
        :y="item.y"
        :w="item.w"
        :h="item.h"
        :i="item.i"
        drag-allow-from=".vue-draggable-handle"
        drag-ignore-from=".no-drag"
        @move="moveEvent"
        @moved="movedEvent"
      >
        <!-- @resize="resizeEvent"
            @resized="resizedEvent"
            @container-resized="containerResizedEvent"
           -->
        <div class="text">
          <div class="vue-draggable-handle" @mousedown="mousedown(inx)" />
          <div class="no-drag">
            <span>{{ item.i }}</span>
            <br>
            <span>{{ item.laboratoryId }}</span>
            <button @click="setLayout">click</button>
          </div>
        </div>
      </grid-item>
    </grid-layout>
  </div>
</template>

<script>
import { GridLayout, GridItem } from 'vue-grid-layout'

export default {
  name: 'Gird',
  components: {
    GridLayout,
    GridItem
  },
  props: {
    colNum: {
      type: Number
      // defet
    },
    maxRows: {
      type: Number
      // defet
    }
  },
  data() {
    return {
      layout: [],
      draggable: true,
      resizable: false,
      activeItem: {
        i: null,
        x1: '',
        y1: '',
        x2: '',
        y2: ''
      }
    }
  },
  created() {
    this.$emit('rendy')
  },
  methods: {
    layoutUpdatedEvent: function(newLayout) {
      // console.log("Updated layout: ", newLayout);
      this.$emit('change', newLayout)
    },
    moveEvent: function() {
      // i, newX, newY
      // console.log(this.layout[i], 88);
      // console.log("moveEvent i=" + i + ", X=" + newX + ", Y=" + newY);
    },
    movedEvent: function(i, newX, newY) {
      // i, newX, newY
      console.log('MOVED i=' + i + ', X=' + newX + ', Y=' + newY)
      if (!this.checkRule(newX, newY)) {
        this.invalidDragHandler()
      }
    },
    mousedown: function(i) {
      const { x, y } = this.layout[i]
      this.activeItem.i = i
      this.activeItem.x1 = x
      this.activeItem.y1 = y
    },
    checkRule(newX) {
      const { i } = this.activeItem
      const { laboratoryId } = this.layout[i]
      // console.log(this.layout[i],"this.layout[i]");
      if (laboratoryId == 1 && newX % 2 != 0) {
        alert('大实验不能放到小实验室中')
        return false
      }
      return true
    },
    invalidDragHandler() {
      const { x1, y1, i } = this.activeItem
      setTimeout(() => {
        this.$set(this.layout[i], 'x', x1)
        this.$set(this.layout[i], 'y', y1)
      }, 0)
    },
    setLayout(layout) {
      // console.log(layout);
      this.layout = [...layout]
      // this.layout.splice(0,this.layout.length)
      // layout.forEach(element => {
      //   this.layout.push(element)
      // });
    }
  }
}
</script>

<style scoped>
.grid-content {
  position: absolute;
  width: 1000px;
  top: 30px;
  left: 80px;
}
.vue-grid-layout {
  /* background: #eee; */
}

.vue-grid-item:not(.vue-grid-placeholder) {
  box-sizing: border-box;
  background: #ccc;
  border: 1px dotted black;
}

.vue-grid-item .resizing {
  opacity: 0.9;
}

.vue-grid-item .static {
  background: #cce;
}

.vue-grid-item .text {
  font-size: 24px;
  text-align: center;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  margin: auto;
  height: 100%;
  width: 100%;
}

.vue-grid-item .no-drag {
  height: 100%;
  width: 100%;
}

.vue-grid-item .minMax {
  font-size: 12px;
}

.vue-grid-item .add {
  cursor: pointer;
}

.vue-draggable-handle {
  position: absolute;
  width: 20px;
  height: 20px;
  top: 0;
  left: 0;
  background: url("data:image/svg+xml;utf8,<svg xmlns='http://www.w3.org/2000/svg' width='10' height='10'><circle cx='5' cy='5' r='5' fill='#999999'/></svg>")
    no-repeat;
  background-position: bottom right;
  padding: 0 8px 8px 0;
  background-repeat: no-repeat;
  background-origin: content-box;
  box-sizing: border-box;
  cursor: pointer;
}
.vue-grid-item .no-drag {
  height: 100%;
  width: 100%;
}
.vue-grid-item .minMax {
  font-size: 12px;
}
.vue-grid-item .add {
  cursor: pointer;
}
.vue-draggable-handle {
  position: absolute;
  width: 20px;
  height: 20px;
  top: 0;
  right: 0;
  padding: 0 8px 8px 0;
  background-origin: content-box;
  background-color: black;
  box-sizing: border-box;
  border-radius: 10px;
  cursor: pointer;
}
</style>
