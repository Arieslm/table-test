<template>
<thead>
    <tr>
        <template v-for="showItem in showColumns">
            <template v-for="(item, index) in tableData.columns" v-if="showItem==item.field">
                <th :style="item.theadStyles">{{item.title}}</th>
            </template>
        </template>
    </tr>
</thead>
</template>
<script type="text/babel">
export default {
    props: ['tableData', 'showColumns', 'fixedNumber'],
    data() {
        return {}
    },
    mounted() {

    }
}
</script>
<style lang="scss" scoped>
thead {
    tr {
        height: 0.6rem;
        th {
            font-size: 0.24rem;
            color: #999999;
            padding: 0 0.16rem;
            width: 1.2rem;
            text-align: right;
            white-space: nowrap;
            background: #f8f8f8;
            border: 1px solid #e5e5e5;
            &:first-child {
                white-space: unset;
                border: none;
                opacity: 0;
            }
        }
    }
}
</style>
