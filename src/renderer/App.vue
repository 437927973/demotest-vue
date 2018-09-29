<template>
    <div id="app">
        <el-container>
            <el-header>
                <!--导航栏-->
                <MenuBar @activeIndex="changeActiveIndex" @search="search"></MenuBar>
            </el-header>
            <el-container>
                <Content :userinfo="tableData" :loading="loading" :chosed="chosed" @chose="choseselection"
                         @pagechange="pagechange" :currentpagenow="currentpage" :totalsize="totalsize"></Content>
            </el-container>
            <el-footer>
                <CustomButtonBar :chosed="chosed" @bill="billing"></CustomButtonBar>
            </el-footer>

            <el-dialog
                    title="开票"
                    :visible.sync="dialogVisible"
                    width="80%"
                    height="70%"
                    append-to-body>
                <span>
                    <ConfirmContent :chosedata="chosed"></ConfirmContent>
                </span>
                <span slot="footer" class="dialog-footer" style="display: inline-block">
                    <label style="margin-right: 10px">总计 : ￥ {{totalMoney}}</label>
                    <el-input id="invoiceinfo"
                              style="width: 200px;"
                              placeholder="请填写单号"
                              v-model="invoicenum">
                    </el-input>
                    <el-button @click="dialogVisible = false" style="margin-left: 10px">取 消</el-button>
                    <el-button type="primary" @click="confirm"
                               v-loading.fullscreen.lock="fullscreenLoading">确 定</el-button>
                </span>
            </el-dialog>
            <el-dialog
                    title="开票信息"
                    :visible.sync="innerVisible"
                    width="80%"
                    height="70%">
                <span>{{billmessage}}</span>
                <span slot="footer" class="dialog-footer">
                    <el-button type="primary" @click="billreturn">确 定</el-button>
                </span>
            </el-dialog>
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
                // showData: [],
                allData: [],
                noInvoiceData: [],
                invoiceData: [],
                searchword: "",
                searchData: [],
                dialogVisible: false,
                errormsg: "",
                invoicenum: "",
                fullscreenLoading: false,
                innerVisible: false,
                billmessage: '',
                isbillsuccess: false,
                currentpage: 1,
                totalsize: 0
            }
        },
        created: function () {
            this.$axios.get("http://localhost:8080/message/userinfo.do", {
            // this.$axios.get("https://www.easy-mock.com/mock/5b909845a93f2b59ad16fb7d/exedemo/userinfo", {
                params: {
                    currentpage: this.currentpage
                }
            })
            // this.$axios.get("http://localhost:8080/demotest/message/userinfo.do")
            // this.$axios.get("http://localhost:8080/message/userinfo.do")
                .then(res => {
                    this.allData = res.data.users;
                    // this.allData.forEach(data => {
                    //     if (data.invoice === "") {
                    //         this.noInvoiceData.push(data)
                    //     } else {
                    //         this.invoiceData.push(data)
                    //     }
                    // });
                    // this.showData = this.allData;
                    this.loading = false;
                    this.totalsize = res.data.totalsize
                }).catch(function (error) {
                this.$message.error('error');
                this.loading = false;
                this.totalsize = 0
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
                return sum.toFixed(2);
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
            pagechange(val) {
                this.currentpage = val;
                if (this.activeIndex === "0") {
                    this.$axios.get("http://localhost:8080/message/userinfo.do", {
                    // this.$axios.get("http://localhost:8080/demotest/message/userinfo.do", {
                    // this.$axios.get("https://www.easy-mock.com/mock/5b909845a93f2b59ad16fb7d/exedemo/userinfo", {
                        params: {
                            currentpage: this.currentpage
                        }
                    }).then(res => {
                        console.log("all change");
                        this.allData = res.data.users;
                        this.totalsize = res.data.totalsize
                    }).catch(error=>{

                    })
                }
                if (this.activeIndex === "1") {
                    // this.$axios.get("https://www.easy-mock.com/mock/5b909845a93f2b59ad16fb7d/exedemo/userinfo", {
                    // this.$axios.get("http://localhost:8080/demotest/message/userinfowithnoinvoice.do", {
                    this.$axios.get("http://localhost:8080/message/userinfowithnoinvoice.do", {
                        params: {
                            currentpage: this.currentpage
                        }
                    }).then(res => {
                        this.noInvoiceData = res.data.users;
                        this.totalsize = res.data.totalsize
                    }).catch(error=>{

                    })
                }
                if (this.activeIndex === "2") {
                    // this.$axios.get("https://www.easy-mock.com/mock/5b909845a93f2b59ad16fb7d/exedemo/userinfo", {
                    // this.$axios.get("http://localhost:8080/demotest/message/userinfowithinvoice.do", {
                    this.$axios.get("http://localhost:8080/message/userinfowithinvoice.do", {
                        params: {
                            currentpage: this.currentpage
                        }
                    }).then(res => {
                        this.invoiceData = res.data.users;
                        this.totalsize = res.data.totalsize
                    }).catch(error=>{

                    })
                }
                if (this.activeIndex === "3") {
                    if(this.searchword == ""){
                        return;
                    }
                    this.$axios.get("http://localhost:8080/message/userinfowithkey.do", {
                    // this.$axios.get("http://localhost:8080/demotest/message/userinfowithkey.do", {
                    // this.$axios.get("https://www.easy-mock.com/mock/5b909845a93f2b59ad16fb7d/exedemo/userinfo", {
                        params: {
                            currentpage: this.currentpage,
                            searchword: this.searchword
                        }
                    }).then(res => {
                        this.invoiceData = res.data.users;
                        this.totalsize = res.data.totalsize
                    }).catch(error=>{

                    })
                }
            },
            billreturn() {
                this.innerVisible = false;
                if (this.isbillsuccess == true) {
                    window.location.reload();
                }
            },
            confirm() {
                if (this.invoicenum == '') {
                    document.getElementById("invoiceinfo").focus()
                    return;
                }
                // this.fullscreenLoading = true;
                // setTimeout(() => {
                //     this.fullscreenLoading = false;
                // }, 2000);

                var chosedId = [];
                this.chosed.forEach(data => {
                    chosedId.push(data.id)
                })
                console.log(chosedId)
                // this.$axios.get("http://localhost:8080/demotest/message/bill.do",{
                this.$axios.get("http://localhost:8080/message/bill.do", {
                    params: {
                        chosedId: encodeURIComponent(JSON.stringify(chosedId)),
                        invoicenum: this.invoicenum
                    }
                }).then(res => {
                    this.billmessage = '开票成功';
                    this.innerVisible = true;
                    this.isbillsuccess = true;
                }).catch(error => {
                    this.billmessage = '开票失败，请检查网络连接后重试';
                    this.innerVisible = true;
                    this.isbillsuccess = false;
                });
            },
            test() {
                console.log(this.chosed)
            },
            choseselection(val) {
                this.chosed = val
            },
            changeActiveIndex(val) {
                this.activeIndex = val;
                this.currentpage = 1;
                this.pagechange(1);
            },
            search(val) {
                this.searchData.splice(0, this.searchData.length);
                this.searchword = val.trim();

                if (this.searchword === "") {
                    this.activeIndex = "0";
                    // this.showData = this.allData;
                    // this.noInvoiceData.splice(0, this.noInvoiceData.length);
                    // this.invoiceData.splice(0, this.invoiceData.length);
                    // this.allData.forEach(data => {
                    //     if (data.invoice === "") {
                    //         this.noInvoiceData.push(data);
                    //     } else {
                    //         this.invoiceData.push(data)
                    //     }
                    // })
                    return
                }


                this.activeIndex = "3";
                // this.allData.forEach(data => {
                //     if ((String(data.dwmc_3)).indexOf(this.searchword) != -1) {
                //         this.searchData.push(data)
                //     }
                // });
                // this.$axios.get("http://localhost:8080/demotest/message/userinfowithkey.do", {
                // this.$axios.get("https://www.easy-mock.com/mock/5b909845a93f2b59ad16fb7d/exedemo/userinfo", {
                this.$axios.get("http://localhost:8080/message/userinfowithkey.do", {
                    params: {
                        currentpage: this.currentpage,
                        searchword: this.searchword
                    }
                }).then(res => {
                    this.searchData = res.data.users;
                    this.totalsize = res.data.totalsize
                })
                // this.showData = this.searchData;
                // this.noInvoiceData.splice(0, this.noInvoiceData.length);
                // this.invoiceData.splice(0, this.invoiceData.length);
                // this.showData.forEach(data => {
                //     if (data.invoice === "") {
                //         this.noInvoiceData.push(data);
                //     } else {
                //         this.invoiceData.push(data)
                //     }
                // })
            },
            billing() {
                if (this.chosed.length == 0) {
                    this.$message.error('还未选中记录');
                    return;
                }
                var hasInvoice = false;
                this.chosed.forEach(data => {
                    if (data.invoice == null || data.invoice == "" ) {
                        hasInvoice = false;
                    }else{
                        hasInvoice = true;
                    }
                    console.log(data.invoice)
                    console.log(hasInvoice)
                });
                if (hasInvoice == true) {
                    this.$message.error('仅未开票记录可选中开票');
                    return;
                }
                var username = this.chosed[0].dwmc_3;
                var nameSameTag = true;
                this.chosed.forEach(data => {
                    if (data.dwmc_3 != username) {
                        nameSameTag = false;
                    }
                });
                if (nameSameTag == false) {
                    this.$message.error('仅相同客户单位可一起选中开票');
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
