<template>
    <div class="wiper-container" 
        ref="wiperbox"
        @touchstart="handleTouchStart"
        @touchend="handleTouchEnd"
        @touchmove="handleTouchMove"
    >
        <div class="wiper-list"
            :style="listStyle"
        >
            <slot :itemHeight="itemHeight">
            </slot>
        </div>
    </div>
</template>

<script>
export default {
    name: 'wiper',
    data () {
        return {
                wait: true, //true显示封面
                pageNum: 1,
                itemHeight: 0,
                position: 1,  //显示可滑动的第几页
                pointChange: 0,
            }
    },
    mounted(){
        this.getItem();
        this.itemHeight = this.$refs.wiperbox.offsetHeight;  //设置每页高度
        const that = this;
        window.onresize = function temp() {
            that.itemHeight = that.$refs.wiperbox.offsetHeight;
        };
        setTimeout(function(){
            that.itemHeight = that.$refs.wiperbox.offsetHeight;
        },1500)
    },
    computed:{
        listStyle(){
            let style = {
                height: this.itemHeight*this.pageNum+'px',
            }
            if(this.pointChange != 0 ){
                //拖动效果
                style.top = this.pageNum == this.position && this.pointChange < 0 || this.position == 1 && this.pointChange > 0 ? 
                            -this.itemHeight*(this.position-1)+'px' 
                            : -this.itemHeight*(this.position-1)+ this.pointChange +'px';
                style.transition = "top 0s";
            }else{
                //滚动翻页效果
                style.top = -this.itemHeight*(this.position-1)+'px';
                style.transition = "top 1s";
            }
            return style
        }
    },
    methods: {
        getItem() {
            let get = this.$slots.default.filter(item => {
                return item.tag != undefined;
            });
            this.pageNum = get.length;
        },
        handleTouchStart: function(e){
            this.startPoint = e.touches[0].pageY;
        },
        handleTouchMove: function(e){
            let poChange = e.changedTouches[0].pageY - this.startPoint;
            this.pointChange = poChange
        },
        handleTouchEnd: function(e){
            let end = e.changedTouches[0].pageY;
            let start = this.startPoint;
            let delta = start - end;
            let goDown = 0;
            if(delta > 10){
                goDown = 1;
            }else if(delta < -10){
                goDown = -1;
            }
            this.changeItem(goDown);
        },
        changeItem: function(down){  //down 大于0 往下滑，小于0 往上滑
            const { position, pageNum } = this;
            this.pointChange = 0;
            if(down > 0 && position >= 1 && position < pageNum ){ //往下滑
                this.position = position + 1;
            }else if(down < 0 && position > 1 && position <= pageNum ){  //往上滑
                this.position = position - 1;
            }
        },
    },
}
</script>

<style lang="scss" scoped="" type="text/css">
    .wiper-container{
        height: 100%;
        width: 100%;
        background: #c4c4c4;
        overflow: hidden;
        position: relative;
        .wiper-list{
            position: absolute;
            width: 100%;
        }
    }
</style>