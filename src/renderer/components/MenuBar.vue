<template>
    <div>
        <el-menu :default-active="activeIndex" class="el-menu-demo" mode="horizontal">
            <el-menu-item index="0" @click="sendActiveIndex('0')">全部</el-menu-item>
            <el-menu-item index="1" @click="sendActiveIndex('1')">未开发票查询</el-menu-item>
            <el-menu-item index="2" @click="sendActiveIndex('2')">已开发票查询</el-menu-item>
            <el-menu-item index="3"><el-input @keyup.enter.native="search" v-model="searchword" placeholder="客户查询"></el-input></el-menu-item>
        </el-menu>
    </div>
</template>
<script>
    export default {
        name: 'MenuBar',
        data() {
            return {
                activeIndex: '0',
                searchword: ""
            }
        },
        computed: {

        },
        methods: {
            sendActiveIndex(val) {
                this.activeIndex = val
                this.$emit("activeIndex", this.activeIndex)
            },
            search(){
                console.log(this.searchword)
                this.$emit("search",this.searchword)
            }
        }
    }

</script>

<style scoped>
    .logo {
        height: 50px;
        width: 50px;
        vertical-align: middle;
        margin-right: 20px;
    }

    .el-col {
        text-align: left;
        vertical-align: middle;
        display: inline-block;
    }

    .title {
        margin-left: 20px;
        margin-right: 20px;
        color: #000000;
    }

    .font_title {
        margin-left: 20px;
        margin-right: 20px;
        color: #000000;
        font-family: "Hiragino Sans GB";
        font-size: 18px;
        font-style: normal;
    }

    .userinfo {
        float: right;
    }
</style>
