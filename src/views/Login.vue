<template>
    <div class="container">

        <div class="row">
            <div class="col-xs-12 col-sm-6 col-md-6">
                <h3>Форма входу</h3>
                <div v-if="loginError.length || passwordError.length" class="alert alert-danger">
                    <strong>
                        {{ loginError || passwordError }}
                    </strong>
                </div>
                <form @submit.prevent="loginSubmit">
                    <div class="form-group">
                        <label for="">Логін</label>
                        <input type="text" v-model="login" class="form-control">
                    </div>
                    <!-- /.form-group -->
                    <div class="form-group">
                        <label for="pwd">Пароль</label>
                        <div id="password_input_box">

                            <input type="password" v-model="password" id="pwd" class="form-control">
                            <div class="icon_box" @click="show_hide_pwd()">
                                <i  class="fas" :class="cssClassIconViewPwd"></i>
                                <span style="padding-left: .6rem; font-size: 1.1rem">Покати/Сховати пароль</span>
                            </div>
                            <!-- /.icon_box -->

                        </div>
                    </div>
                    <!-- /.form-group -->
                    <div class="form-group">
                        <button class="btn btn-success" >Увійти</button>
                        <!-- /.btn -->
                    </div>
                    <!-- /.form-group -->
                </form>
            </div>
            <!-- /.col-xs-12 col-sm-6 col-md-6 -->
            <div class="col-xs-12 col-sm-6 col-md-6">
                <h3>Реєстрація в сисьниі</h3>
            </div>
            <!-- /.col-xs-12 col-sm-6 col-md-6 -->
        </div>
        <!-- /.row -->
    </div>
    <!-- /.container -->
</template>
<script>
    import {load} from 'recaptcha-v3'
    import axios from 'axios'



    export default {
        name: 'Login',
        data(){
            return {
                cssClassIconViewPwd: 'fa-eye',
                loginError: '',
                passwordError: '',
                login: '',
                password: '',
                tokenRecaptcha: null
            }
        },
        mounted() {
            const reCaptchaSiteKey = '6Lf8QMMZAAAAANQ5bJLTBRFhaFwhhIXf9y_DDcb1'
            load(reCaptchaSiteKey).then(recaptcha => {
                recaptcha.execute('test').then(token => {
                    this.tokenRecaptcha = token
                })
            })

        },
        watch:{
            login:function(){
                this.validate_login()
            },
            password: function(){
                this.validate_password()
            }
        },
        methods:{
            loginSubmit(){
                this.validate_login()
                this.validate_password()
                if (this.loginError.length || this.passwordError.length){
                    return
                }else{
                    axios.post('http://localhost', {
                        type: 'login',
                        tokenRecaptcha: this.tokenRecaptcha,
                        login: this.login,
                        password: this.password
                    }).then(response => {
                        const resp = response.data
                        if (resp.isLogin){
                            this.$router.push('/')
                        } else{
                            this.loginError = 'Помилка входу!'
                            this.login = ''
                            this.password = ''
                        }
                    })
                }
            },
            show_hide_pwd(){
                const password_input = document.getElementById('pwd')
                if (password_input.type === 'password'){
                    password_input.type = 'text'
                    this.cssClassIconViewPwd = 'fa-eye-slash'
                }else{
                    password_input.type = 'password'
                    this.cssClassIconViewPwd = 'fa-eye'
                }
            },
            validate_password(){
                const password = this.password
                if (password.length < 8){
                    this.passwordError = 'Короткий пароль, не маньше 8-ми символів!'
                }else {
                    this.passwordError = ''
                }
            },
            validate_login(){
                const login = this.login
                const pt_reg = new RegExp('^[a-zA-Z0-9]+$','i')
                if (login.length < 3){
                    this.loginError = 'Короткий логін'
                }else if (!pt_reg.test(login)){
                    this.loginError = 'Некоректний логін, тільки букви латинського алфавіту та цифри'
                }else{
                    this.loginError = ''
                }
            }
        }
    }
</script>

<style lang="scss">
    #password_input_box{
        /*display: flex;*/
    }
    .icon_box{
        margin-top: 1rem;
        font-size: 2rem;
    }
</style>