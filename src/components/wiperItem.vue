<template>
    <div class="wiper-item" :style="itemStyle">
        <slot></slot>
    </div>
</template>

<script>
export default {
    name: "wiper-item",
    data() {
        return {
          itemStyle: ""
        };
    },
    props: ['color'],
    mounted() {
        this.$nextTick(()=>{
            this.itemStyle = {
                height: this.$options.parent.itemHeight + "px",
                background: this.color,
            }
        })
    },
    methods: {
      
    },
    watch: {
       
    }
};
</script>

<style lang="scss" scoped="" type="text/css">
    .wiper-item{
        width: 100%;
        display: flex;
        flex-direction: column;
        justify-content: space-around;
        div{
          width: 100%;
        }
    }
</style>