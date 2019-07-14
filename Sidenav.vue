<template>
    <div ref="sidenav" class="sidenav" v-bind:class="{'sidenav-open':value===true,'sidenav-close':value===false}">
        <div class="content">
            <slot></slot>
        </div>
        <div ref="background" class="background">

        </div>
    </div>
</template>

<script>
    export default {
        name: "Sidenav",
        props:['value'],
        mounted(){
            this.$refs.background.addEventListener('click',()=>{ this.$emit('input',false); });

            document.addEventListener('touchstart',(e)=>{
                this.touchOrigin = [{x:e.touches[0].screenX,y:e.touches[0].screenY}]
            });

            document.addEventListener('touchmove',(e)=>{
                this.touchOrigin.push({x:e.touches[0].screenX,y:e.touches[0].screenY});

                var slide = this.computeSlide();
                if (slide>0 && !this.value || slide<0 && this.value==true)
                    this.$refs.sidenav.style.left = slide+"px";

            });

            document.addEventListener('touchend',()=>{
                this.$refs.sidenav.style.left = "0";

                var slide = this.computeSlide();
                if (slide>window.innerWidth/4)
                    this.$emit("input",true);
                if (slide<-window.innerWidth/4)
                    this.$emit("input",false);

                this.touchOrigin = [];
            });
        },
        data(){
            return {
                touchOrigin:null
            }
        },
        methods:{
            computeSlide(){
                let x=0;
                let y=0;
                for (let i = 1; i<this.touchOrigin.length; i++){
                    x+=this.touchOrigin[i].x-this.touchOrigin[i-1].x;
                    y+=this.touchOrigin[i].y-this.touchOrigin[i-1].y;
                }
                if (Math.abs(x)>Math.abs(y)){
                    let slide = this.touchOrigin[this.touchOrigin.length-1].x-this.touchOrigin[0].x;
                    if (slide>280) slide = 280;
                    if (slide<-280) slide = -280;
                    return slide;
                }
                return 0;
            }
        }
    }
</script>

<style scoped>
    .sidenav{
        position: fixed;
        display: flex !important;
        flex-direction: row;
        top: 0;
        left: 0;
        transform: translateX(-280px);
        width: 100%;
        height: 100%;
        z-index: 1000;
        transition-duration: .5s;
        pointer-events: none; /* On mobile devices, .background is in the forefront */
    }

    .sidenav .content{
        width: 280px;
        height: 100%;
        background-color: #fff;
    }

    .sidenav .background{
        width: calc(100% - 280px);
        height: 100%;
        /*background-color: rgb(255,0,0,.5);*/
    }

    .sidenav-close{
        transform: translateX(-280px);
    }

    .sidenav-open{
        transform: translateX(0);
    }
    .sidenav.sidenav-open{
        transform: translateX(0);
        pointer-events: all;
    }

    .sidenav.sidenav-open .content{
        box-shadow: 0 0 20px #222;
    }


    @media (min-width: 768px) {

        .sidenav{
            width: 280px;
        }

        .sidenav .content{
            width: 100%;
        }

        .sidenav .background{
            width: 0;
        }

    }
</style>
