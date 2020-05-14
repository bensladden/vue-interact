<template>
  <div :id="'item' + id" ref="item" class="item" :style="cssStyle">
    <!-- Main Content Slot -->
    <slot></slot>
    <!-- Resize Top Div -->
    <div
      :id="id + '-resizeTop'"
      :ref="id + '-resizeTop'"
      class="resize resize-top"
      :style="{
        height: resizeHandleSize + 'px',
        top: -(resizeHandleSize / 2) + 'px',
        left: 0 + 'px',
        right: 0 + 'px',
        cursor: 'ns-resize',
        position: 'absolute'
      }"
      v-if="resizeTop"
    >
      <slot name="resizeTop"></slot>
    </div>
    <!-- Resize Bottom Div -->
    <div
      :id="id + '-resizeBottom'"
      :ref="id + '-resizeBottom'"
      class="resize resize-bottom"
      :style="{
        height: resizeHandleSize + 'px',
        left: 0 + 'px',
        right: 0 + 'px',
        bottom: -(resizeHandleSize / 2) + 'px',
        cursor: 'ns-resize',
        position: 'absolute'
      }"
      v-if="resizeBottom"
    >
      <slot name="resizeBottom"></slot>
    </div>
    <!-- Resize Left Div -->
    <div
      :id="id + '-resizeLeft'"
      :ref="id + '-resizeLeft'"
      class="resize resize-left"
      :style="{
        width: resizeHandleSize + 'px',
        top: 0 + 'px',
        bottom: 0 + 'px',
        left: -(resizeHandleSize / 2) + 'px',
        cursor: 'ew-resize',
        position: 'absolute'
      }"
      v-if="resizeLeft"
    >
      <slot name="resizeLeft"></slot>
    </div>
    <!-- Resize Right Div -->
    <div
      :id="id + '-resizeRight'"
      :ref="id + '-resizeRight'"
      class="resize resize-right"
      :style="{
        width: resizeHandleSize + 'px',
        top: 0 + 'px',
        bottom: 0 + 'px',
        right: -(resizeHandleSize / 2) + 'px',
        cursor: 'ew-resize',
        position: 'absolute'
      }"
      v-if="resizeRight"
    >
      <slot name="resizeRight"></slot>
    </div>
    <!-- Top Left Resize Div -->
    <div
      :id="id + '-resizeTopLeft'"
      :ref="id + '-resizeTopLeft'"
      class="resize resize-left resize-top"
      :style="{
        width: resizeHandleSize * 2 + 'px',
        height: resizeHandleSize * 2 + 'px',
        top: -resizeHandleSize + 'px',
        left: -resizeHandleSize + 'px',
        cursor: 'nw-resize',
        position: 'absolute'
      }"
      v-if="resizeTopLeft"
    >
      <slot name="resizeTopLeft"></slot>
    </div>
    <!-- Top Right Resize Div -->
    <div
      :id="id + '-resizeTopRight'"
      :ref="id + '-resizeTopRight'"
      class="resize resize-right resize-top"
      :style="{
        width: resizeHandleSize * 2 + 'px',
        height: resizeHandleSize * 2 + 'px',
        top: -resizeHandleSize + 'px',
        right: -resizeHandleSize + 'px',
        cursor: 'ne-resize',
        position: 'absolute'
      }"
      v-if="resizeTopRight"
    >
      <slot name="resizeTopRight"></slot>
    </div>
    <!-- Bottom Right Resize Div -->
    <div
      :id="id + '-resizeBottomRight'"
      :ref="id + '-resizeBottomRight'"
      class="resize resize-right resize-bottom"
      :style="{
        width: resizeHandleSize * 2 + 'px',
        height: resizeHandleSize * 2 + 'px',
        bottom: -resizeHandleSize + 'px',
        right: -resizeHandleSize + 'px',
        cursor: 'nw-resize',
        position: 'absolute'
      }"
      v-if="resizeBottomRight"
    >
      <slot name="resizeBottomRight"></slot>
    </div>

    <!-- Bottom Left Resize Div -->
    <div
      :id="id + '-resizeBottomLeft'"
      :ref="id + '-resizeBottomLeft'"
      class="resize resize-left resize-bottom"
      :style="{
        width: resizeHandleSize * 2 + 'px',
        height: resizeHandleSize * 2 + 'px',
        bottom: -resizeHandleSize + 'px',
        left: -resizeHandleSize + 'px',
        cursor: 'ne-resize',
        position: 'absolute'
      }"
      v-if="resizeBottomLeft"
    >
      <slot name="resizeBottomLeft"></slot>
    </div>
  </div>
</template>

<script>
import "@interactjs/actions";
import "@interactjs/auto-start";

import interact from "@interactjs/interact";

export default {
  props: {
    id: { type: [Number, String], required: true },
    draggable: { type: Boolean, default: true },
    resizable: { type: Boolean, default: true },
    resizeEdges: { type: String, default: "bottom right" },
    resizeHandleSize: { type: Number, default: 8 },
    resizeHold: { type: Number, default: 0 },
    dragHold: { type: Number, default: 0 }
  },
  data() {
    return {
      interactInstance: null,
      x: 0,
      y: 0,
      top: 0,
      left: 0,
      widthPx: 200,
      heightPx: 200
    };
  },
  methods: {
    setDraggable() {
      if (this.draggable) {
        this.interactInstance.draggable({
          enabled: true,
          hold: this.dragHold,
          listeners: {
            start: event => {
              this.onMoveStart(event);
            },
            move: event => {
              this.onMove(event);
            },
            end: event => {
              this.onMoveEnd(event);
            }
          }
        });
      } else {
        this.interactInstance.draggable(false);
      }
    },
    setResizable() {
      if (this.resizable) {
        this.interactInstance.resizable({
          enabled: true,
          hold: this.resizeHold,
          edges: {
            top: ".resize-top",
            left: ".resize-left",
            bottom: ".resize-bottom",
            right: ".resize-right"
          },
          listeners: {
            start: event => {
              this.onResizeStart(event);
            },
            move: event => {
              this.onResize(event);
            },
            end: event => {
              this.onResizeEnd(event);
            }
          }
        });
      } else {
        this.interactInstance.resizable(false);
      }
    },
    onMoveStart(event) {
      console.log(event);
    },
    onMove(event) {
      this.left += event.dx;
      this.top += event.dy;
    },
    onMoveEnd(event) {
      console.log(event);
    },
    onResizeStart(event) {
      console.log(event);
    },
    onResize(event) {
      this.left += event.deltaRect.left;
      this.top += event.deltaRect.top;
      this.widthPx = event.rect.width;
      this.heightPx = event.rect.height;
    },
    onResizeEnd(event) {
      console.log(event);
    }
  },
  computed: {
    cssStyle() {
      // return {
      //   top: this.top + "px",
      //   left: this.left + "px",
      //   width: this.widthPx + "px",
      //   height: this.heightPx + "px"
      // };
      const translate =
        "translate3d(" + this.left + "px," + this.top + "px, 0)";
      return {
        transform: translate,
        WebkitTransform: translate,
        MozTransform: translate,
        msTransform: translate,
        OTransform: translate,
        width: this.widthPx + "px",
        height: this.heightPx + "px"
      };
    },
    resizeTop() {
      return this.resizable && this.resizeEdges.includes("top");
    },
    resizeBottom() {
      return this.resizable && this.resizeEdges.includes("bottom");
    },
    resizeLeft() {
      return this.resizable && this.resizeEdges.includes("left");
    },
    resizeRight() {
      return this.resizable && this.resizeEdges.includes("right");
    },
    resizeTopLeft() {
      return this.resizeTop && this.resizeLeft;
    },
    resizeBottomLeft() {
      return this.resizeBottom && this.resizeLeft;
    },
    resizeTopRight() {
      return this.resizeTop && this.resizeRight;
    },
    resizeBottomRight() {
      return this.resizeBottom && this.resizeRight;
    }
  },
  watch: {
    draggable() {
      this.setDraggable();
    },
    resizable() {
      this.setResizable();
    }
  },
  mounted() {
    this.interactInstance = interact(this.$refs.item);
    this.setDraggable();
    this.setResizable();
  },
  beforeDestroy() {
    this.interactInstance.unset();
  }
};
</script>

<style>
.item {
  box-sizing: border-box;
  position: absolute;
  display: inline-block;
  transition: all 200ms ease;
  transition-property: left, top, right;
  touch-action: none;
  user-select: none;
}

.resize {
  touch-action: none;
  user-select: none;
}
</style>
