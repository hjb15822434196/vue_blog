<template>
    <div>
        <el-container>
            <el-header>
                <router-link to="/blogs">
                       <img src="https://www.markerhub.com/dist/images/logo/markerhub-logo.png"
                             style="height: 60%; margin-top: 10px;"> 
                </router-link>
            </el-header>

            <el-main>
                <el-form  :model="ruleForm" status-icon :rules="rules" ref="ruleForm" label-width="80px"
                          class="demo-ruleForm">
                    <el-form-item label="用户名" prop="username" >
                        <el-input type="text" maxlength="12" v-model="ruleForm.username"></el-input>
                    </el-form-item>
                    <el-form-item label="密码" prop="password" >
                        <el-input type="password" v-model="ruleForm.password" autocomplete="off"></el-input>
                    </el-form-item>
                    <el-form-item>
                        <el-button type="primary" @click="submitForm('ruleForm')">登录</el-button>
                        <el-button @click="resetForm('ruleForm')">重置</el-button>
                    </el-form-item>
                </el-form>
            </el-main>
        </el-container>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                ruleForm: {
                    username: '郝嘉宾',
                    password: '111111'
                },
                rules: {
                    password: [
                        {required: true, message: '请输入密码', trigger: 'blur'},
                        {min: 3, max: 12, message: '长度在 3 到 12 个字符', trigger: 'blur'}
                    ],
                    username: [
                        {required: true, message: '请输入用户名', trigger: 'blur'},
                        {min: 3, max: 12, message: '长度在 3 到 12 个字符', trigger: 'blur'}
                    ]
                }
            }

        },
        methods: {
                submitForm(formName) {
                    const _this = this //全局变量
                    this.$refs[formName].validate((valid) => {
                        if (valid) {
                           this.$axios.post('/login',this.ruleForm).then(res =>{
                               const token = res.headers['authorization']
                               const userInfo =res.data.data
                               //把数据共享出去
                               _this.$store.commit('SET_TOKEN', token)
                               _this.$store.commit('SET_USERINFO', userInfo)
                               _this.$router.push("/blogs")
                           })
                        } else {
                            console.log('error submit!!');
                            return false;
                        }
                    });
                },
                resetForm(formName) {
                    this.$refs[formName].resetFields();
                }

        }
    }
</script>

<style scoped="less">
    .el-header, .el-footer {
        background-color: #B3C0D1;
        color: #333;
        text-align: center;
        line-height: 60px;
    }
    .el-main {
        background-color: white;
        text-align: center;
        line-height: 160px;
    }

    body > .el-container {
        margin-bottom: 40px;
    }

    .el-container:nth-child(5) .el-aside,
    .el-container:nth-child(6) .el-aside {
        line-height: 260px;
    }

    .el-container:nth-child(7) .el-aside {
        line-height: 320px;
    }
    .demo-ruleForm{
        margin: 0 auto;
        width: 500px;
    }
</style>