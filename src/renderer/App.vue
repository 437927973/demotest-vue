<template>
    <div id="app">
        <el-container>
            <el-header>
                <!--导航栏-->
                <MenuBar @activeIndex="changeActiveIndex" @search="search"></MenuBar>
            </el-header>
            <el-container>
                <Content :userinfo="tableData" :loading="loading" :chosed="chosed" @chose="choseselection"></Content>
            </el-container>
            <el-footer>
                <CustomButtonBar :chosed="chosed" @bill="billing"></CustomButtonBar>
            </el-footer>

            <el-dialog
                    title="开票"
                    :visible.sync="dialogVisible"
                    width="80%"
                    height="70%">
                <!--<span>这是一段信息</span>-->
                <span>
                    <ConfirmContent :chosedata="chosed"></ConfirmContent>
                </span>
                <span slot="footer" class="dialog-footer" style="display: inline-block">
                    <label style="margin-right: 30px">总计 : ￥ {{totalMoney}}</label>
                    <el-input id="invoiceinfo"
                              style="width: 200px;"
                              placeholder="请填写单号"
                              v-model="invoiceinfo">
                    </el-input>
                    <el-button @click="dialogVisible = false">取 消</el-button>
                    <el-button type="primary" @click="confirm"
                               v-loading.fullscreen.lock="fullscreenLoading">确 定</el-button>
                </span>
            </el-dialog>
            <!--<el-button :plain="true" @click="open3">警告</el-button>-->
        </el-container>
    </div>
</template>

<script>
    import MenuBar from './components/MenuBar'
    import Content from './components/Content'
    import CustomButtonBar from './components/CustomButtonBar'
    import ConfirmContent from './components/ConfirmContent'

    export default {
        name: 'my-project',
        components: {
            MenuBar,
            Content,
            CustomButtonBar,
            ConfirmContent
        },
        data() {
            return {
                loading: true,
                chosed: [],
                activeIndex: "0",
                allData: [],
                noInvoiceData: [],
                invoiceData: [],
                searchword: "",
                searchData: [],
                dialogVisible: false,
                errormsg: "",
                invoiceinfo: "",
                fullscreenLoading: false
            }
        },
        created: function () {
            this.$axios.get("https://www.easy-mock.com/mock/5b909845a93f2b59ad16fb7d/exedemo/userinfo")
                .then(res => {
                    this.allData = res.data.users;
                    this.allData.forEach(data => {
                        if (data.invoice === "") {
                            this.noInvoiceData.push(data)
                        } else {
                            this.invoiceData.push(data)
                        }
                    })
                    this.loading = false
                }).catch(function (error) {
                this.$message.error('error');
                return;
            })
        },
        computed: {
            totalMoney() {
                var sum = 0;
                if (this.chosed.length == 0) {
                    return sum;
                }
                this.chosed.forEach(user => {
                    sum += Number(user.sl_1) * Number(user.je_);
                })
                return sum;
            },
            tableData() {
                if (this.activeIndex == "0") {
                    return this.allData;
                }
                if (this.activeIndex == "1") {
                    return this.noInvoiceData;
                }
                if (this.activeIndex == "2") {
                    return this.invoiceData;
                }
                if (this.activeIndex == "3") {
                    return this.searchData;
                }
            }
        },
        methods: {
            confirm() {
                if (this.invoiceinfo == '') {
                    document.getElementById("invoiceinfo").focus()
                    return;
                }
                this.fullscreenLoading = true;
                setTimeout(() => {
                    this.fullscreenLoading = false;
                }, 2000);
                this.dialogVisible = false
            },
            test() {
                console.log(this.chosed)
            },
            choseselection(val) {
                this.chosed = val
            },
            changeActiveIndex(val) {
                this.activeIndex = val;
            },
            search(val) {
                console.log("hello")
                this.searchData.splice(0, this.searchData.length)
                this.searchword = val

                if (this.searchword === "") {
                    this.activeIndex = "0"
                    return
                }


                this.activeIndex = "3"
                this.allData.forEach(data => {
                    if ((String(data.username)).indexOf(this.searchword) != -1) {
                        this.searchData.push(data)
                    }
                })
            },
            billing() {
                if (this.chosed.length == 0) {
                    this.$message.error('还未选中记录');
                    return;
                }
                var noInvoice = true;
                this.chosed.forEach(data => {
                    if (data.invoice != "") {
                        noInvoice = false;
                    }
                })
                if (noInvoice == false) {
                    this.$message.error('仅未开票记录可选中开票');
                    return;
                }
                this.dialogVisible = true;
            }
        }
    }
</script>

<style>
    /* CSS */
</style>
