<template>
    <div class="hello">
        <wiper>
            <wiperitem color="#90F7EC">
                <div>123</div>
                <div>123</div>
                <div>123</div>
                <div>123</div>
            </wiperitem>
            <wiperitem color="#7CAAFF">
                <span>456</span>
            </wiperitem>
        </wiper>
    </div>
</template>

<script>
import Wiper from '../components/wiper'
import WiperItem from '../components/wiperItem'
export default {
    name: 'home',
    components: {
        wiper: Wiper,
        wiperitem: WiperItem,
    },
    data () {
        return {
          msg: ''
        }
    },
    mounted(){

    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped="" type="text/css">
    .hello{
        height: 100%;
        width: 100%;
    }
</style>
