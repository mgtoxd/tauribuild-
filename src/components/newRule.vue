<template>
  <div v-show="showRec" class="border-4 z-50 border-red-600 border-solid absolute" @mouseup="end"  :style="'top: '+pageStartXY[1]+'px;left: '+pageStartXY[0]+'px;height:'+(pageEndXY[1]-pageStartXY[1])+'px;width:'+(pageEndXY[0]-pageStartXY[0])+'px'"></div>
  <el-button color="#535353" type="primary" @click="changeDraw">返回</el-button>
  <el-image
      @mouseup="end"
      @mousedown="begin"
      @mousemove="moving"

      @dragstart="dragMethod"
      draggable="false"
      :src="'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRuCWLU2eyfAoeVpgOfz06nCBYduv4wQB0cbtk8SCU&usqp=CAU'"
      :fit="'contain'"
      style="width: 100%"
      ref="desktop">

  </el-image>



  <el-dialog
      v-model="dialogVisible"
      title="确认选择"
      width="30%"
  >
    <el-row>
      <el-col :span="8">
        <el-image
            draggable="false"
            :src="'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRuCWLU2eyfAoeVpgOfz06nCBYduv4wQB0cbtk8SCU&usqp=CAU'"
            :fit="'contain'"
            style="width: 100%"
        >

        </el-image>
      </el-col>
      <el-col :span="8">
        <el-checkbox v-model="palette" label="调色" size="large"></el-checkbox>
      </el-col>
      <el-col :span="8">
        <el-color-picker @change="convertColor" v-if="palette" v-model="paletteBound" />
      </el-col>
    </el-row>
    <template #footer>
      <span class="dialog-footer">
        <el-button @click="dialogVisible = false">Cancel</el-button>
        <el-button type="primary" @click="this.dialogVisible = false;"
        >确定</el-button>
      </span>
    </template>
  </el-dialog>

</template>


<script>
export default {
  props: ["visible", "changeDraw"],
  name: "newRule",
  created() {
    // this.$refs.desktop.addEventListener('mouseup', () => {
    //   console.log('dodododododod')
    //   this.choosing = false
    // })
  },
  data() {
    return {
      paletteBound:'',
      palette:false,
      dialogVisible:false,
      showRec:false,
      choosing: false,
      pageStartXY: [0, 0],

      pageEndXY: [0, 0]
    }
  },
  methods: {
    dragMethod(e){
      e.preventDefault()
    },
    convertColor(){

      console.log(this.colorRgb(this.paletteBound))
    },
    colorRgb( sColor){
      sColor = sColor.toLowerCase();
      //十六进制颜色值的正则表达式
      var reg = /^#([0-9a-fA-f]{3}|[0-9a-fA-f]{6})$/;
      // 如果是16进制颜色
      if (sColor && reg.test(sColor)) {
        if (sColor.length === 4) {
          var sColorNew = "#";
          for (var i=1; i<4; i+=1) {
            sColorNew += sColor.slice(i, i+1).concat(sColor.slice(i, i+1));
          }
          sColor = sColorNew;
        }
        //处理六位的颜色值
        var sColorChange = [];
        for (var i=1; i<7; i+=2) {
          sColorChange.push(parseInt("0x"+sColor.slice(i, i+2)));
        }
        return "RGB(" + sColorChange.join(",") + ")";
      }
      return sColor;
    },
    moving(e) {
      if (this.choosing) {
        this.pageEndXY[0] = e.pageX
        this.pageEndXY[1] = e.pageY
      }
    },
    begin(e) {
      this.showRec = true
      this.choosing = true
      this.pageStartXY[0] = e.pageX
      this.pageStartXY[1] = e.pageY
      this.pageEndXY[0] = e.pageX
      this.pageEndXY[1] = e.pageY

    },
    end(e) {
      this.choosing = false
      this.dialogVisible = true
    }
  }
}
</script>

<style scoped>

</style>