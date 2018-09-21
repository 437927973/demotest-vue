<template>
    <el-table
            ref="table"
            :data="chosedata"
            height="350px"
            stripe
            style="width: 100%;margin:10px 5px 15px 20px;">
        <el-table-column
                type="index"
                label="序号"
                width="50">
        </el-table-column>
        <el-table-column
                prop="fphm_"
                label="销售单号"
                width="80">
        </el-table-column>
        <el-table-column
                label="日期"
                width="180">
            <template slot-scope="scope">
                <i class="el-icon-time"></i>
                <span style="margin-left: 10px">{{ scope.row.fhrq_ }}</span>
            </template>
        </el-table-column>
        <el-table-column
                prop="dwmc_3"
                label="客户单位">
        </el-table-column>
        <el-table-column
                prop="pm_"
                label="品名">
        </el-table-column>
        <el-table-column
                prop="gg_"
                label="规格">
        </el-table-column>
        <el-table-column
                prop="sl_1"
                label="数量">
        </el-table-column>
        <el-table-column
                prop="sl_2"
                label="重量">
        </el-table-column>
        <el-table-column
                prop="dj_"
                label="含税单价">
        </el-table-column>
        <el-table-column
                prop="je_"
                label="销售金额">
        </el-table-column>
        <el-table-column
                prop="gfdb_"
                label="业务员">
        </el-table-column>
    </el-table>
</template>

<script>
    export default {
        name: "ConfirmContent",
        props: ['chosedata'],
        data() {
            return {
            }
        },
        methods: {
        }
    }
</script>

<style scoped>

</style>