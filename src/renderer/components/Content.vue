<template>
    <!--<div>-->
    <el-table
            ref="table"
            v-loading="loading"
            :data="userinfo"
            height="520px"
            stripe
            style="width: 100%;margin:10px 5px 15px 20px;"
            @selection-change="handleSelectionChange"
            @row-click="rowClick">
        <el-table-column
                type="selection">
        </el-table-column>
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
                label="规格"
                width="80">
        </el-table-column>
        <el-table-column
                prop="sl_1"
                label="数量"
                width="80">
        </el-table-column>
        <el-table-column
                prop="sl_2"
                label="重量"
                width="100">
        </el-table-column>
        <el-table-column
                prop="dj_"
                label="含税单价"
                width="100">
        </el-table-column>
        <el-table-column
                prop="je_"
                label="销售金额">
        </el-table-column>
        <el-table-column
                prop="gfdb_"
                label="业务员">
        </el-table-column>
        <el-table-column
                label="是否已开发票"
                width="150">
            <template slot-scope="scope">
                <span style="margin-left: 10px">{{ scope.row.invoice === "" ? "无" : scope.row.invoice}}</span>
            </template>
        </el-table-column>
    </el-table>
    <!--</div>-->
</template>

<script>
    export default {
        name: "Content",
        props: ['userinfo', 'chosed','loading'],
        data() {
            return {
                selection: [],
            }
        },
        methods: {
            handleSelectionChange(val) {
                this.selection = val;
                this.$emit('chose', this.selection)
            },
            rowClick(row, event, column) {
                this.$refs.table.toggleRowSelection(row);
            }
        }
    }
</script>

<style scoped>

</style>
