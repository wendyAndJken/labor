<template>
  <div
    class="calendar"
    style="
        position: relative;
        padding-left: 80px;
        border: 1px solid rgb(219, 219, 219);
      "
  >
    <div class="Courses-head" style="overflow: hidden">
      <div
        v-for="(itm, index) in dateList"
        :key="index"
        class="Courses-head-1"
        style="
            float: left;
            box-sizing: border-box;
            white-space: nowrap;
            width: 200px;
            border-right: 1px solid #ccc
          "
      >
        {{ itm }}
      </div>
    </div>
    <div class="Courses-content">
      <ul
        v-for="(itm, index) in timeList"
        :key="index"
        class="stage_1"
        style="list-style: none; padding: 0px; margin: 0px; min-height: 20px"
      >
        <li
          v-for="(item, inx) in dateList"
          :key="inx"
          class=""
          style="
              float: left;
              width: 100px;
              height: 20px;
              box-sizing: border-box;
              position: relative;
              border-right: 1px solid #ccc
            "
        />
        <li
          v-for="(item, inx) in dateList"
          :key="`${inx}gg`"
          class=""
          style="
              float: left;
              width: 100px;
              height: 20px;
              box-sizing: border-box;
              position: relative;
              border-right: 1px solid #ccc
            "
        />
      </ul>
    </div>
    <div
      class="Courses-leftHand"
      style="position: absolute; left: 0px; top: 0px; width: 80px"
    >
      <div
        class="left-hand-TextDom"
        style="height: 29px; box-sizing: border-box"
      />
      <div
        v-for="(item, inx) in timeList"
        :key="inx"
        class="left-hand-1"
        style="width: 100%; height: 20px; box-sizing: border-box"
      >
        <p class="left-hand-name" style="margin: 0px">{{ item }}</p>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Calendar',
  props: {
    dateList: {
      type: Array,
      default: () => []
    },
    timeList: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {}
  },
  methods: {}
}
</script>
<style scoped>

/* .courseWrapper */
.Courses-head {
  background-color: #edffff;
}

.Courses-head > div {
  text-align: center;
  font-size: 14px;
  line-height: 28px;
}

.left-hand-TextDom,
.Courses-head {
  background-color: #f2f6f7;
}

.Courses-leftHand {
  background-color: #f2f6f7;
  font-size: 12px;
}

.Courses-leftHand .left-hand-index {
  color: #9c9c9c;
  margin-bottom: 4px !important;
}

.Courses-leftHand .left-hand-name {
  color: #666;
}

.Courses-leftHand p {
  text-align: center;
  font-weight: 900;
}

.Courses-head > div {
  border-left: none !important;
}

.Courses-leftHand > div {
  /* padding-top: 5px; */
  border-bottom: 1px dashed rgb(219, 219, 219);
}

.Courses-leftHand > div:last-child {
  border-bottom: none !important;
}

.left-hand-TextDom,
.Courses-head {
  border-bottom: 1px solid rgba(0, 0, 0, 0.1) !important;
}

.Courses-content > ul {
  border-bottom: 1px dashed rgb(219, 219, 219);
  box-sizing: border-box;
}

.Courses-content > ul:last-child {
  border-bottom: none !important;
}

.highlight-week {
  color: #02a9f5 !important;
}

.Courses-content li {
  text-align: center;
  color: #666666;
  font-size: 14px;
  line-height: 20px;
}

.Courses-content li span {
  padding: 6px 2px;
  box-sizing: border-box;
  line-height: 18px;
  border-radius: 4px;
  white-space: normal;
  word-break: break-all;
  cursor: pointer;
}

.grid-active {
  z-index: 9999;
}

.grid-active span {
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.2);
}
</style>
