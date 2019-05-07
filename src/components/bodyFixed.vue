<template>
<tbody>
    <template v-for="(item,indexs) in tableData.data">
            <tr :style="item.styles">
                <template v-for="(key,index) in tableData.columns">
                    <td v-show="index < fixedNumber" :data-col="index" :data-row="indexs" class="normal-td" :style="item.important ? item.styles : key.styles">
                         <!-- <span v-if="item[key.field + '_jump_func']" @click="item[key.field + '_jump_func'](index,indexs,item)" >{{item[key.field]}}</span> -->
                          {{item[key.field]}}
                    </td>
                </template>
            </tr>
    </template>
</tbody>
</template>
<script type="text/babel">
export default {
    props: ['tableData', 'showColumns', 'fixedNumber'],
    data() {
        return {
            // fixedNumber: 1
        }
    },
    mounted() {
    },
    // computed: {
    //     styles(col, row) {
    //         return function () {
    //             // console.log('col:', col, 'row:', row);
    //             let pos = [0, 5];
    //             this.dataStyles(showColumns, col, pos);
    //             let styles = {color: 'red'};
    //             return styles
    //         }
    //     }
    // },
    methods: {
        dataStyles(data, col, pos) {
            console.log(data, col, pos);
            // data.forEach(item, index) {
            //     if (item.style && )
            // }
        }
    }
}
</script>
<style lang="scss" scoped>
.bgred {
    background: red;
}
tbody {
    tr {
        border: 1px solid #e5e5e5;
        height: 0.8rem;
        overflow: hidden;
        font-size: 0.24rem;
        td {
            padding: 0.05rem 0.22rem;
            text-align: right;
            width: 1.2rem;
            overflow: hidden;
            white-space: nowrap;
            border: 1px solid #e5e5e5;
            &:first-child {
                word-break: break-all;
                white-space: unset;
            }
        }
    }
}
</style>
