<template>
    <div>
        <div
                class="btn-box"
                ref="cool_nav"
                :style="`
                left:${mouseX}px;
                top:${mouseY}px;
                `"
        >
            <slot/>
        </div>
        <h1>{{mouseX}}</h1>
        <h1>{{mouseY}}</h1>
    </div>
</template>

<script>
    export default {
        name: "coolNav",
        data(){
            return{
                window_Width:0,
                window_height:0,
                mouseStart_time:0,
                mouseEnd_time:0,
                mouseOn_time:0,
                mouseX:0,
                mouseY:0,
                ifMouseDown:false,
            }
        },
        props: {
        },
        methods:{
            mouseDown(e){
                this.mouseStart_time = e.timeStamp;
                this.ifMouseDown=true;

                const Cool_Nav = this.$refs.cool_nav;
                Cool_Nav.addEventListener("mousemove",this.mouseMove);
                Cool_Nav.addEventListener("touchmove",this.mouseMove);

                console.log('按下了',e);
            },
            mouseMove(e){
                if(e.type=='mousemove' && this.ifMouseDown){
                    this.mouseX=e.pageX;
                    this.mouseY=e.pageY;
                }
                if(e.type=='touchmove' && this.ifMouseDown){
                    this.mouseX=e.touches[0].pageX;
                    this.mouseY=e.touches[0].pageY;
                }
                console.log(e);
            },
            mouseUp(e){
                this.ifMouseDown=false;
                this.mouseEnd_time = e.timeStamp;
                const Cool_Nav = this.$refs.cool_nav;
                Cool_Nav.removeEventListener("mousemove",this.mouseMove);
                Cool_Nav.removeEventListener("touchmove",this.mouseMove);
                console.log('松开了',e);
                this.mouseOn_time = this.mouseEnd_time - this.mouseStart_time;
                // console.log(this.mouseOn_time);
                if (this.mouseOn_time>200){
                    this.mouseLongPress();
                }else if(this.mouseOn_time<=200 && this.mouseOn_time>0) {
                    this.mouseShortPress();
                }
            },
            mouseOut(e){
                this.ifMouseDown=false;
                const Cool_Nav = this.$refs.cool_nav;
                Cool_Nav.removeEventListener("mousemove",this.mouseMove);
                Cool_Nav.removeEventListener("touchmove",this.mouseMove);
            },
            mouseShortPress(){
                alert('点击')
            },
            mouseLongPress(){
                console.log('长按');
            },
            init() {
                this.window_Width=window.innerWidth;
                this.window_height=window.innerHeight;
                this.mouseX=this.window_Width/2;
                this.mouseY=this.window_height-50;
                console.log(window.innerWidth,window.innerHeight);
            },
        },
        created(){
            this.init();
        },
        mounted() {
            this.$nextTick(()=>{
                const Cool_Nav = this.$refs.cool_nav;
                Cool_Nav.addEventListener("mousedown",this.mouseDown);
                Cool_Nav.addEventListener("touchstart",this.mouseDown);

                Cool_Nav.addEventListener("mouseup",this.mouseUp);
                Cool_Nav.addEventListener("touchend",this.mouseUp);

                // Cool_Nav.addEventListener("mousemove",this.mouseMove);
                // Cool_Nav.addEventListener("touchmove",this.mouseMove);

                Cool_Nav.addEventListener("mouseout",this.mouseOut);
                Cool_Nav.addEventListener("touchcancel",this.mouseOut);
            });
        }
    }
</script>

<style scoped>
.btn-box{
    position: fixed;
    transform: translate(-50%,-50%);
    width: 100px;
    height: 100px;
    border-radius: 100%;
    background: #f00;
    text-align: center;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
}
.btn-box-new{
    z-index: 999;
    position: fixed;
    transform: translate(-50%,-50%);
    width: 200px;
    height:200px;
    border-radius: 100%;
    background: #00f;
    text-align: center;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
}
</style>
