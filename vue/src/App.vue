<template>
    <div id="app">
        <div>
            <el-main>
                <el-row :gutter="20">
                    <el-col :span="16">
                        <div class="grid-content bg-purple">
                            <h1>
                                指令详情
                            </h1>
                            <el-tabs v-model="activeName" @tab-click="handleClick">
                                <el-tab-pane label="指令状态" name="first">
                                    <el-table
                                            :data="insTable"
                                            border
                                            style="width: 100%">
                                        <el-table-column
                                                prop="instruction"
                                                label="指令">
                                        </el-table-column>
                                        <el-table-column
                                                prop="target"
                                                label="target">
                                        </el-table-column>
                                        <el-table-column
                                                prop="j"
                                                label="j">
                                        </el-table-column>
                                        <el-table-column
                                                prop="k"
                                                label="k">
                                        </el-table-column>
                                        <el-table-column
                                            prop="issue"
                                            label="issue">
                                        </el-table-column>
                                        <el-table-column
                                                prop="readOperand"
                                                label="readOperand">
                                        </el-table-column>
                                        <el-table-column
                                            prop="exeComplet"
                                            label="exeComplet">
                                        </el-table-column>
                                        <el-table-column
                                            prop="writeResult"
                                            label="writeResult">
                                    </el-table-column>
                                    </el-table>
                                </el-tab-pane>
                                <el-tab-pane label="功能单元状态" name="second">
                                    <el-table
                                            :data="funcUTable"
                                            border
                                            style="width: 100%">
                                        <el-table-column
                                                prop="name"
                                                label="name">
                                        </el-table-column>
                                        <el-table-column
                                                prop="busy"
                                                label="busy">
                                        </el-table-column>
                                        <el-table-column
                                                prop="Op"
                                                label="Op">
                                        </el-table-column>
                                        <el-table-column
                                                prop="Fi"
                                                label="Fi（目的寄存器）">
                                        </el-table-column>
                                        <el-table-column
                                                prop="Fj"
                                                label="Fj（源寄存器）">
                                        </el-table-column>
                                        <el-table-column
                                                prop="Fk"
                                                label="Fk（源寄存器）">
                                        </el-table-column>
                                        <el-table-column
                                                prop="Qj"
                                                label="Qj">
                                        </el-table-column>
                                        <el-table-column
                                                prop="Qk"
                                                label="Qk">
                                        </el-table-column>
                                        <el-table-column
                                                prop="Rj"
                                                label="Rj">
                                        </el-table-column>
                                        <el-table-column
                                                prop="Rk"
                                                label="Rk">
                                        </el-table-column>
                                    </el-table>

                                </el-tab-pane>
                                <el-tab-pane label="寄存器状态" name="third">
                                    <el-table
                                            :data="registerTable"
                                            border
                                            style="width: 100%">
                                        <el-table-column
                                                prop="F0"
                                                label="F0">
                                        </el-table-column>
                                        <el-table-column
                                                prop="F1"
                                                label="F1">
                                        </el-table-column>
                                        <el-table-column
                                                prop="F2"
                                                label="F2">
                                        </el-table-column>
                                        <el-table-column
                                                prop="F3"
                                                label="F3">
                                        </el-table-column>
                                        <el-table-column
                                                prop="F4"
                                                label="F4">
                                        </el-table-column>
                                        <el-table-column
                                                prop="F5"
                                                label="F5">
                                        </el-table-column>
                                        <el-table-column
                                                prop="F6"
                                                label="F6">
                                        </el-table-column>
                                        <el-table-column
                                                prop="F7"
                                                label="F7">
                                        </el-table-column>
                                        <el-table-column
                                                prop="F8"
                                                label="F8">
                                        </el-table-column>
                                        <el-table-column
                                                prop="F9"
                                                label="F9">
                                        </el-table-column>
                                        <el-table-column
                                                prop="F10"
                                                label="F10">
                                        </el-table-column>
                                        <el-table-column
                                                prop="F11"
                                                label="11">
                                        </el-table-column>
                                    </el-table>
                                </el-tab-pane>
                            </el-tabs>
                        </div>
                    </el-col>
                    <el-col :span="8">
                        <div class="grid-content bg-purple">
                            <h1>操作</h1>
                            <el-form  label-width="80px">
                                <el-form-item label="当前周期">
                                    <el-input :value="this.current" :disabled="true"></el-input>
                                </el-form-item>


                                <el-form-item label="指令">
                                    <el-input
                                            type="textarea"
                                            :rows="2"
                                            placeholder="输入指令内容，每条指令之间换行"
                                            v-model="input">
                                    </el-input>
                                </el-form-item>


                                <el-form-item label="">
                                    <el-button type="primary" plain v-on:click="getdata(input,cycler)">提交指令</el-button>
                                </el-form-item>

                                <el-form-item label="选择周期">
                                    <el-input v-model="cycler" value="1"></el-input>
                                </el-form-item>
                                <el-form-item label="">
                                    <el-button type="primary" plain v-on:click="getdata(input,cycler)">提交周期</el-button>
                                </el-form-item>
                            </el-form>


                        </div>
                    </el-col>
                </el-row>
            </el-main>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';

    export default {
        name: 'app',
        components: {},
        data() {
            return {
                activeName: 'second',
                current: "0",
                input: "",
                cycler: "0",
                insTable:[],
                funcUTable:[],
                registerTable:[]
            }
        },
        methods: {
            handleClick(tab, event) {
                // console.warn(tab, event);
                console.warn("Log a warn level message.");

            },
            getdata(str,cycler) {
                console.log(str);
                console.log(cycler);
                this.activeName = "first";
                let tempstr = str.replace(/\+/g, "%2B");
                tempstr = tempstr.replace(new RegExp("\n", "gm"), "  ");
                console.log(tempstr);
                this.current = cycler;
                axios.get('http://127.0.0.1:9999/input?input='+tempstr+"&cycler="+cycler).then((response) => {
                    let data = response.data;
                    this.insTable = data.insTable;
                    this.funcUTable = data.funcUTable;
                    this.registerTable = data.registerTable
                }).catch((response) => {
                    console.log(response)
                })
            }
        }
    }


</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
</style>
