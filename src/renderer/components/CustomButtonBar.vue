<template>
    <div style="float: right;margin-right: 20px;margin-bottom: 5px">
        <label style="margin-right: 30px">总计 : ￥ {{totalMoney}}</label>
        <el-button type="primary" @click="billing">开 票</el-button>
    </div>
</template>

<script>
    export default {
        name: "CustomButtonBar",
        props: ['chosed'],
        data() {
            return {}
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
            }
        },
        methods: {
            billing() {
                console.log(this.chosed);
                this.$emit("bill")
            }
        }
    }
</script>

<style scoped>

</style>
