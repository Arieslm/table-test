<template>
    <div class="table" v-if="domShow">
        <div v-if="domShow">
            <!-- 表头 -->
            <table class="left-table">
                <head-fixed :showColumns="showColumns" :tableData="sexSaleData" :fixedNumber="fixedNumber"></head-fixed>
                <body-fixed :showColumns="showColumns" :tableData="sexSaleData" :fixedNumber="fixedNumber"></body-fixed>
            </table>
            <div class="scroll-table ScrollY" ref="tabletest">
                <!-- 表格内容 -->
                <table class="right-table" ref="table">
                    <thead-detail ref="tableHead" :tableData="sexSaleData" :showColumns="showColumns" :fixedNumber="fixedNumber"></thead-detail>
                <template>
                      <tbody-detail :tableData="sexSaleData" :showColumns="showColumns" :fixedNumber="fixedNumber"></tbody-detail>
                  </template>
                </table>
           </div>
        </div>
    </div>
</template>
<script>
import headFixed from '../components/headFixed';
import bodyFixed from '../components/bodyFixed';
import theadDetail from '../components/headDetail';
import tbodyDetail from '../components/bodyDetail';
import {timeSaleData, areaSaleData, sexSaleData, activitySaleData} from './table.js'
export default{
    components: {
        headFixed,
        bodyFixed,
        tbodyDetail,
        theadDetail
    },
    data(){
        return{
            isAndroid: false,
            isIos: false,
            fixedNumber: '1',
            timeSaleData: timeSaleData,
            areaSaleData: areaSaleData,
            sexSaleData: sexSaleData,
            activitySaleData: activitySaleData,
            areaSaleDataColumns: [],
            sexSaleDataColumns: [],
            showColumns: [],
            activitySaleDataColumns: [],
            domShow: false
        }
    },
    mounted(){
        this.areaSaleDataColumns = this.areaSaleData.columns.map((item)=>{
            return item.field
        })
        this.showColumns = this.sexSaleData.columns.map((item)=>{
            return item.field
        })
        this.activitySaleDataColumns = this.activitySaleData.columns.map((item)=>{
            return item.field
        })
        this.domShow = true;
        this.$nextTick(()=>{
            this.init();
        })
    },
    methods: {
        init(){
            // this.initAttr();

            if(this.isAndroid){
                // console.log('go')
                let scrollObj = this.scrollObj || document;
                let curObj = this.$refs.table; // 当前元素
                let curW = curObj.offsetWidth;
                let curHead = this.$refs.tableHead;
                let curdis = this.$refs.jiange;
                let curT = 0;
                let curH = 0;
                let scrollT = 0;

                scrollObj.addEventListener('scroll',()=>{
                    curT = this.getTop(curObj);
                    curH = curObj.offsetHeight;
                    if(scrollObj == document){
                        scrollT = document.documentElement.scrollTop || document.body.scrollTop;
                    }else{
                        scrollT = scrollObj.scrollTop;
                    }
                    if(scrollT >= curT && scrollT <= curT+curH){
                        curHead.style.position = 'fixed';
                        curHead.style.top = 0;
                        curHead.style.left = '50%';
                        curHead.style.width = curW + 'px';
                        curHead.style.webkitTransform = 'translateX(-50%)';
                        curdis.style.display = 'block';
                    }else{
                        curHead.style.position = 'relative';
                        curHead.style.top = 0;
                        curHead.style.left = 0;
                        curHead.style.webkitTransform = 'translateX(0)';
                        curdis.style.display = 'none';
                    }
                    // console.log('t:',curT,'h:',curH,'s:',scrollT)
                })
            }
            // 拖拽交互
            let  boxW = this.$refs.table.offsetWidth;
            let wrapW = this.$refs.tabletest.offsetWidth;
            let isDrag = false;
            // let headObj = this.$refs.table.children[0];
            // let dragObj = this.$refs.table.children[1];
            let scrollDom = this.$refs.table;
            let startX = 0;
            let translateX = 0;
            let startPointX = 0;
            let startPointY = 0;
            let isFirst = true;
            let isMove = true;
            let disX = 0;
            let disY = 0;
            let dis = 0;
            if(boxW - wrapW > 0){
                isDrag = true;
            }else{
                isDrag = false;
            }
            if(!isDrag) return;
            scrollDom.addEventListener('touchstart',(ev)=>{
                start(ev)
            })

            scrollDom.addEventListener('touchmove',(ev)=>{
                move(ev)
            })

            function start(ev){
                // console.log('go')
                var touch = ev.changedTouches[0];
                startX = translateX;
                startPointX = touch.pageX;
                startPointY = touch.pageY;
                isFirst = true;
                isMove = true;
            }

            function move(ev){
                var touch = ev.changedTouches[0];
                disX = touch.pageX - startPointX;
                disY = touch.pageY - startPointY;
                dis = 2*Math.abs(disX) - Math.abs(disY);
                translateX = disX + startX;

                if(isFirst){
                    isFirst = false;

                    if(dis < 0){
                        isMove = false;
                    }
                }

                if(isMove){
                    ev.preventDefault();
                    // console.log(translateX);
                    if(translateX > 0){
                        translateX = 0;
                    }else if(translateX < wrapW - boxW){
                        translateX = wrapW - boxW;
                    }
                    scrollDom.style.webkitTransform = `translateX(${translateX}px)`;
                    // dragObj.style.webkitTransform = `translateX(${translateX}px)`;
                }
            }
        },
    }
}
</script>
<style lang="scss" scoped>
.no-table {
    opacity: 0;
    border: none !important;
}
.table {
    background: #fff;
    .left-table {
        // float: left;
        background: #fff;
        position: absolute;
        // table-layout: fixed;
        box-shadow: 0 0 7px 0 rgba(0, 0, 0, 0.16);
        z-index: 99;
    }
    .right-table {
        width: max-content;
        // table-layout: fixed;
    }
}
.ScrollY {
    overflow-x: hidden;
    -webkit-overflow-scrolling: touch;
}
.ScrollN {
    overflow: hidden;
}
</style>
