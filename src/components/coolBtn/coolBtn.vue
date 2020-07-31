<template>
  <div
      ref="pageDiv"
      @mousemove = "move"
      @touchmove  = "move"
      @mouseup   = "up"
      @touchend  = "up"
      :class="{'coolBtn_page':mouseDownState}"
  >
    <div class="coolBtn_bg"></div>
    <div
        :class="{'coolBtn-base':true,'coolBtn':!menuOpen,'coolBtn-open':menuOpen}"
        ref="actionMgr"
        @mousedown="down"
        @touchstart="down"
        :style="position"
    >
      <div class="Btn" @click="click">
        导航
      </div>
      <div :class="{'coolBtn-menu-base':true,'coolBtn_menu':!menuOpen,'coolBtn_menu-open':menuOpen}">
        <div>首页</div>
        <div>首页</div>
        <div>首页</div>
        <div>首页</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "coolBtn",
  data() {
    return {
      menuOpen:false,     //  菜单展开状态
      mouseDownState:false,   //  鼠标点击状态
      iX:0,
      iY:0,
      dx:0,
      dy:0,
      lastMoveIndex:0,    //  拖拽计数
      curMoveIndex:0, //  历史计数
    }
  },
  props: {
    // 通过position来设置初始定位
    position: {
      type: Object,
      default: function() {
        return {
          top: "32.25rem",
          left: "0"
        }
      }
    }
  },
  methods:{
    down(e){
      //  如果打开了菜单，则不做响应
      if(this.menuOpen){
        this.mouseDownState = false;
        return
      }
      /* 此处判断  pc 或 移动端 得到 e 事件 */
      var touch;
      if (e.touches) {
        touch = e.touches[0];
      } else {
        touch = e;
      }

      // 鼠标点击 面向页面 的 x坐标 y坐标
      let { clientX, clientY } = touch;
      // 鼠标x坐标 - 拖拽按钮x坐标  得到鼠标 距离 拖拽按钮 的间距
      this.iX = clientX - this.$refs.actionMgr.offsetLeft;
      // 鼠标y坐标 - 拖拽按钮y坐标  得到鼠标 距离 拖拽按钮 的间距
      this.iY = clientY - this.$refs.actionMgr.offsetTop;

      // 设置当前 状态为 鼠标按下
      this.mouseDownState = true;
    },
    move(e){
      //鼠标按下 切移动中
      if (this.mouseDownState) {
        /* 此处判断  pc 或 移动端 得到 e 事件 */
        var touch;
        if (e.touches) {
          touch = e.touches[0];
        } else {
          touch = e;
        }
        // 鼠标移动时 面向页面 的 x坐标 y坐标
        let { clientX, clientY } = touch;
        //当前页面全局容器 dom 元素  获取容器 宽高
        let {clientHeight: pageDivY,clientWidth: pageDivX} = this.$refs.pageDiv;
        /* 鼠标坐标 - 鼠标与拖拽按钮的 间距坐标  得到 拖拽按钮的 左上角 x轴y轴坐标 */
        let [x, y] = [clientX - this.iX, clientY - this.iY];
        let {clientHeight: actionMgrY,clientWidth: actionMgrX,style:actionMgrStyle} = this.$refs.actionMgr;

        if (x > pageDivX - actionMgrX) x = pageDivX - actionMgrX;
        else if (x < 0) x = 0;
        if (y > pageDivY - actionMgrY) y = pageDivY - actionMgrY;
        else if (y < 0) y = 0;
        // 计算后坐标  设置 按钮位置
        actionMgrStyle.left = `${x}px`;
        actionMgrStyle.top = `${y}px`;
        actionMgrStyle.bottom = "auto";
        actionMgrStyle.right = "auto";

        this.lastMoveIndex++;
        //当按下键滑动时， 阻止屏幕滑动事件
        e.preventDefault();
      }
    },
    up(e){
      this.mouseDownState = false;
      if(e.touches){
        this.curMoveIndex = this.lastMoveIndex;
      }
    },
    click(e){
      console.log("click",this.lastMoveIndex,this.curMoveIndex);
      if(this.lastMoveIndex-this.curMoveIndex<=10){
        this.menuOpen = !this.menuOpen;
      }
      this.curMoveIndex = this.lastMoveIndex;
    },
  }
}
</script>

<style scoped>
.coolBtn_page{
  z-index: 9999;
  position:fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(255,0,0,0.2);
}

.coolBtn-base{
  z-index: 9999;
  position: fixed;
  width: 60px;
  height: 60px;
  border-radius: 100%;
  line-height: 60px;
  background-color: #337AB7;
  text-align: center;
  color: #fff;
  cursor: pointer;
}
.coolBtn{
}
.coolBtn-open{
}
.Btn{
  z-index:999;
}

.coolBtn-menu-base{
  visibility: hidden;
}
.coolBtn_menu{
}
.coolBtn_menu-open{
  visibility: unset;
  background: #97a8be;
}
</style>