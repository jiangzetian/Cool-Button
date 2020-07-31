<template>
    <div class="cool-btn">
        <div
                ref="cool_nav"
                :class="`
                  btn-box-base
                  ${ifOpen?'btn-box-open':'btn-box'}
                `"
                :style="`
                  left:${mouseX}px;
                  top:${mouseY}px;
                  width: ${btnWidth}px;
                  height: ${btnWidth}px;
                `"
        >
            <slot class="content" />
        </div>
        <ul
            :class="`
                  btn-menu-base
                  ${ifOpen?'btn-menu-open':'btn-menu'}
                `"
            :style="`
                  left:${mouseX}px;
                  top:${mouseY}px;
            `"
        >
          <slot name="btnListItem"></slot>
        </ul>
    </div>
</template>

<script>
    export default {
        name: "coolNav",
        data(){
            return{
                mouseStart_time:0,
                mouseEnd_time:0,
                mouseOn_time:0,
                mouseX:0,
                mouseY:0,
                ifMouseDown:false,
                ifOpen:false,
            }
        },
        props: {
          left:{
            type:Number,
            require:true,//指定是否为必须
            default:0,//如果没有传入值时默认是初始值
          },
          top:{
            type:Number,
            require:true,//指定是否为必须
            default:0,//如果没有传入值时默认是初始值
          },
          btnWidth:{
            type:Number,
            require:true,//指定是否为必须
            default:60,//如果没有传入值时默认是初始值
          },
          btnHeight:{
            type:Number,
            require:true,//指定是否为必须
            default:60,//如果没有传入值时默认是初始值
          },
        },
        methods:{
            //初始化数据
            init() {
              this.mouseX=this.left;
              this.mouseY=this.top;
            },
            //按下
            mouseDown(e){
                this.mouseStart_time = e.timeStamp;
                this.ifMouseDown=true;

                const Cool_Nav = this.$refs.cool_nav;
                Cool_Nav.addEventListener("mousemove",this.mouseMove);
                Cool_Nav.addEventListener("touchmove",this.mouseMove);

                // console.log('按下了',e);
            },
            //移动
            mouseMove(e){
                if(e.type=='mousemove' && this.ifMouseDown){
                    this.mouseX=e.pageX;
                    this.mouseY=e.pageY;
                }
                if(e.type=='touchmove' && this.ifMouseDown){
                    this.mouseX=e.touches[0].pageX;
                    this.mouseY=e.touches[0].pageY;
                }
                // console.log(e);
            },
            //松开
            mouseUp(e){
                this.ifMouseDown=false;
                this.mouseEnd_time = e.timeStamp;
                const Cool_Nav = this.$refs.cool_nav;
                Cool_Nav.removeEventListener("mousemove",this.mouseMove);
                Cool_Nav.removeEventListener("touchmove",this.mouseMove);
                // console.log('松开了',e);
                this.mouseOn_time = this.mouseEnd_time - this.mouseStart_time;
                // console.log(this.mouseOn_time);
                if (this.mouseOn_time>150){
                    this.mouseLongPress();
                }else if(this.mouseOn_time<=150 && this.mouseOn_time>0) {
                    this.mouseShortPress();
                }
            },
            //移出
            mouseOut(e){
                this.ifMouseDown=false;
                const Cool_Nav = this.$refs.cool_nav;
                Cool_Nav.removeEventListener("mousemove",this.mouseMove);
                Cool_Nav.removeEventListener("touchmove",this.mouseMove);
            },
            //短按事件
            mouseShortPress(){
              console.log(this.ifOpen);
              if (this.ifOpen){
              }
              this.ifOpen = !this.ifOpen;
              this.$emit('mouseShortPress');
            },
            //长按事件
            mouseLongPress(){
                this.$emit('mouseLongPress');
            },
        },
        created(){
            this.init();
        },
        mounted() {
            //事件监听
            this.$nextTick(()=>{
                const Cool_Nav = this.$refs.cool_nav;
                Cool_Nav.addEventListener("mousedown",this.mouseDown);
                Cool_Nav.addEventListener("touchstart",this.mouseDown);

                Cool_Nav.addEventListener("mouseup",this.mouseUp);
                Cool_Nav.addEventListener("touchend",this.mouseUp);

                // Cool_Nav.addEventListener("mouseout",this.mouseOut);
                // Cool_Nav.addEventListener("touchcancel",this.mouseOut);
            });
        }
    }
</script>

<style scoped>
.btn-box-base{
  z-index: 999;

  position: fixed;
  transform: translate(-50%,-50%);
  text-align: center;

  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}
.btn-box{
  transition:transform .6s;
  border-radius: 100%;
  background: #ff9900;
  color: #fff;
  font-weight: 500;
  box-shadow: 2px 2px 5px #888888;
  opacity:0.7 ;
}
.btn-box-open{
  transition:transform .6s;
  transform:translate(-50%,-50%) scale(1.1);
  border-radius: 100%;
  background: #ff9900;
  color: #fff;
  font-weight: 500;
  box-shadow: 0px 0px 10px #888888;
}
.btn-box-base li{
  display: none;
}
.btn-box-base span{
  font-size: 26px;
  position: relative;
}
.btn-box-base span:before{
  content: "";
  display: block;
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  /*background: #ff0000;*/
}

.btn-menu-base{
  z-index: 888;
  box-sizing:border-box;
  visibility: hidden;
  position: fixed;
  width:60px;
  padding:0;
  margin: 0;
  font-size: 16px;
  list-style: none;
  border-radius: 10px;
}
.btn-menu{
  height: 0;
  font-size: 0;
  transform: translate(-50%,-100%);
}
.btn-menu-open{
  visibility: unset;
  position: fixed;
  transform: translate(-50%,-100%);
  height:180px;
  padding-bottom:30px;
  background: #ff9900;
  color: #fff;
  text-align: center;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  transition:height .8s;
}


.btn-menu-open li{
  margin:10px 0;
}
.content{
  width: 60px;
  height: 60px;
  background: #f00;
}
</style>
