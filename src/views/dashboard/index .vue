<template>
  <div>
    <div class="mainBox">
      <!-- <Calendar :dateList="dateList" :timeList="timeList" />
      <Gird
        :colNum="dateList.length * 2"
        :maxRows="timeList.length"
        ref="girdRef"
        @rendy="onGirdRendy"
        @change="onGirdChange"
      /> -->
    </div>
    <button @click="push">填充</button>
  </div>
</template>

<script>
// import Calendar from "./components/Calendar.vue";
// import Gird from "./components/Gird.vue";

export default {
  name: 'GirdCalendar',
  // components: { Calendar, Gird },
  props: {},
  data() {
    return {
      dateList: ['周一', '周二', '周三', '周四', '周五'],
      timeList: [
        '8:00-8:30', '8:30-9:00',
        '9:00-9:30', '9:30-10:00',
        '10:00-10:30', '10:30-11:00',
        '11:00-11:30', '11:30-12:00',
        '12:00-12:30', '12:30-13:00',
        '13:00-13:30', '13:30-14:00',
        '14:00-14:30', '14:30-15:00',
        '15:00-15:30', '15:30-16:00',
        '16:00-16:30', '16:30-17:00',
        '17:00-17:30', '17:30-18:00',
        '18:00-18:30', '18:30-19:00',
        '19:00-19:30', '19:30-20:00',
        '20:00-20:30', '20:30-21:00',
        '21:00-21:30', '21:30-22:00',
        '22:00-22:30', '22:30-23:00',
        '23:00-23:30', '23:30-00:00',
        '00:00-00:30', '00:30-1:00',
        '1:00-1:30', '1:30-2:00',
        '2:00-2:30', '2:30-3:00',
        '3:00-3:30', '3:30-4:00',
        '4:00-4:30', '4:30-5:00',
        '5:00-5:30', '5:30-6:00',
        '6:00-6:30', '6:30-7:00',
        '7:00-7:30', '7:30-8:00'
      ],
      laboratorys: [
        {
          id: 1,
          name: '大实验室'
        },
        {
          id: 2,
          name: '小实验室'
        }
      ],
      configData: {
        bg: {
          type: 'a1',
          houers: 3,
          name: '大实验1'
        },
        min: [
          {
            type: 'a1',
            houers: 1,
            name: '小实验2'
          },
          {
            type: 'a2',
            houers: 2,
            name: '小实验3'
          },
          {
            type: 'a2',
            houers: 2,
            name: '小实验3'
          },
          {
            type: 'a2',
            houers: 2,
            name: '小实验3'
          },
          {
            type: 'a2',
            houers: 2,
            name: '小实验3'
          },
          {
            type: 'a2',
            houers: 2,
            name: '小实验3'
          },
          {
            type: 'a2',
            houers: 2,
            name: '小实验3'
          }
        ]
      },
      grids: [],
      list: [
        { d: '周一', t: '8:00-8:30', h: 1, i: '9', laboratoryId: 2 },
        { d: '周三', t: '8:00-8:30', h: 2, i: '1', laboratoryId: 2 },
        { d: '周一', t: '8:00-8:30', h: 5, i: '2', laboratoryId: 1 },
        { d: '周三', t: '13:30-14:00', h: 1, i: '19', laboratoryId: 2 }
      ],
      layouts: []
    }
  },
  created() {
    this.listToLayout()
    this.initGrids()
    // this.setGrid()
  },

  methods: {
    listToLayout() {
      const layouts = []
      this.list.forEach((it, inx) => {
        const { d, t, h, laboratoryId } = it
        const inxD = this.dateList.indexOf(d)
        const inxL = this.laboratorys.findIndex((itm) => itm.id == laboratoryId)
        const x = inxD * this.laboratorys.length + inxL
        const y = this.timeList.indexOf(t)
        layouts.push({
          x,
          y,
          h,
          w: 1,
          static: false,
          i: inx,
          laboratoryId
        })
      })
      this.layouts = layouts
      console.log(this.layouts, 'this.layouts')
    },
    initGrids() {
      const grids = []
      // 初始化网格
      this.dateList.forEach((date, index) => {
        this.laboratorys.forEach((laboratory, inx) => {
          const xGrids = []
          this.timeList.forEach((tiem, i) => {
            xGrids.push({
              tiem,
              date,
              laboratory,
              status: 'blank',
              x: index * this.laboratorys.length + inx,
              y: i
            })
          })
          grids.push(xGrids)
        })
      })
      this.grids = grids
    },
    updateGrids() {
      // 给网格添加状态
      const grids = this.grids
      this.layouts.forEach((it) => {
        const { h, x, y } = it
        for (let y1 = 0; y1 < h; y1++) {
          const y2 = y + y1
          grids[x][y2].status = 'full'
        }
      })
      // 给网格添加最大长度
      grids.forEach((it, inx) => {
        let maxBlank = 0
        for (let index = 0; index < it.length; index++) {
          const reverseIndex = it.length - index - 1
          const item = it[reverseIndex]
          if (item.status === 'blank') {
            maxBlank++
          } else {
            maxBlank = 0
          }
          grids[inx][reverseIndex].maxBlank = maxBlank
        }
      })
      this.grids = grids
      console.log(this.grids, 'this.grids')
    },
    push() {
      let flag = false
      // 往大实验室填充大实验
      flag = this.setBghours()
      if (flag) return
      // 往小实验室填充小实验
      flag = this.setMinHors('min')
      if (!flag) return
      // 往大实验室填充小实验
      flag = this.setMinHors('bg')
      if (flag) return
    },
    setMinHors(type) {
      let bgLaboratoryFlag = false
      let bgMall = 0
      const minLay = []
      let mh = 0
      this.configData.min.forEach(it => {
        if (typeof (it.houers * 1) !== 'number') return
        bgMall = it.houers + bgMall
        minLay.push({
          h: it.houers,
          w: 1,
          mh: mh
        })
        mh = mh + it.houers
      })
      // let bx = this.configData.bg.x;
      const by = this.configData.bg.y
      const by1 = this.configData.bg.y + this.configData.bg.h
      this.grids.forEach((it) => {
        if (bgLaboratoryFlag) return
        it.forEach((item, inx) => {
          // 在大实验只能在大实验室中进行，
          const laboratoryId = type == 'min' ? 2 : 1
          if (item.maxBlank >= bgMall && (item.laboratory.id == laboratoryId) && !bgLaboratoryFlag) {
            const { x, y, laboratory, date } = item
            const y1 = y + bgMall
            // debugger // eslint-disable-line
            if (date == this.configData.bg.date && !(by1 <= y || by >= y1)) {
              return
            }
            minLay.forEach((itm, inxde) => {
              this.layouts.push({
                x,
                y: y + itm.mh,
                h: itm.h,
                w: 1,
                static: false,
                i: `${new Date().getTime()}-${inx}-${inxde}`,
                laboratoryId: laboratory.id
              })
            })
            bgLaboratoryFlag = true
          }
        })
      })
      // let
      if (!bgLaboratoryFlag) {
        console.log('新实验室失败')
        return true
      }
      this.setGrid()
    },
    setBghours() {
      let bgLaboratoryFlag = false
      this.grids.forEach((it) => {
        if (bgLaboratoryFlag) return
        it.forEach((item, inx) => {
          const bgM = this.configData.bg.houers
          // 在大实验只能在大实验室中进行，
          if (item.maxBlank >= bgM && item.laboratory.id == 1 && !bgLaboratoryFlag) {
            const { x, y, laboratory, date } = item
            this.layouts.push({
              x,
              y: y,
              h: bgM,
              w: 1,
              static: false,
              i: `${new Date().getTime()}-${inx}`,
              laboratoryId: laboratory.id
            })
            this.configData.bg.x = x
            this.configData.bg.h = bgM
            this.configData.bg.y = y
            this.configData.bg.date = date
            bgLaboratoryFlag = true
          }
        })
      })
      // let
      if (!bgLaboratoryFlag) {
        console.log('新实验室失败')
        return true
      }
      // return ;
      this.setGrid()
    },
    onGirdRendy() {
      setTimeout(() => {
        this.setGrid()
      }, 500)
    },
    onGirdChange(newLoay) {
      if (newLoay && newLoay.length > 0) {
        this.layouts = [...newLoay]
      }
    },
    setGrid() {
      this.updateGrids()
      console.log(this.layouts, 'this.layouts')
      this.$refs.girdRef?.setLayout(this.layouts)
    }
  }
}
</script>
<style scoped>
.wrapper {
  display: flex;
  justify-content: center;
  width: 100%;
}
.item {
  width: 300px;
  height: 20px;
  background-color: #42b983;
  color: #ffffff;
}
.item2 {
  height: 100px;
}
</style>
<style>
.mainBox {
  /* padding: 15px 10px; */
  width: 1100px;
  margin: 30px auto;
  position: relative;
}
</style>
