<template>
    <div class="about">

        <vs-row vs-justify="center">
            <h1 style="padding: 0 1rem">ورود به برنامه</h1>
            <p style="text-align: justify; padding: 1rem 2rem; color: #cfc100">
                <strong style="color: orange;">نکته مهم!</strong>
                <br>
                اگر اولین بار است که برنامه را اجرا می کنید، رمزی که وارد می کنید در سیستم ثبت شده و غیر قابل تغییر است! برای ورود های بعدی نیز باید از همان رمز عبور استفاده کنید.
            </p>
            <vs-col type="flex" vs-justify="center" vs-align="center"
                    vs-w="12"
                    style="padding: 1em 2em">
                <vs-card>
                    <div slot="header" v-if="hasError">
                        <span style="color: red">
                            رمز عبور اشتباه است!
                        </span>
                    </div>
                    <div>
                        <div style="font-size: 32px; padding-bottom: 1rem;">
                            <vs-input class="inputx" style="width: 100%" size="large" label="رمز عبور"
                                      placeholder="رمز عبور خود را وارد کنید"
                                      v-model="pwd"/>
                        </div>
                    </div>
                    <div slot="footer">
                        <vs-row vs-justify="flex-end">
                            <vs-button color="success" @click="login" icon="save" class="vs-divider">ورود
                            </vs-button>
                        </vs-row>
                    </div>
                </vs-card>
            </vs-col>
        </vs-row>
    </div>
</template>
<script>
    import router from './../router/index';
    import md5 from 'md5';

    export default {
        name: 'Login',
        data: () => ({
            password: null,
            pwd: null,
            hasError: false
        }),
        methods: {
            login() {
                if (localStorage.getItem('password')) {
                    if (localStorage.getItem('password') === md5(this.pwd)) {
                        this.hasError = false;
                        router.push('/home')
                    } else {
                        this.hasError = true;
                    }
                } else {
                    localStorage.setItem('password', md5(this.pwd));
                    router.push('/home')
                }
            }
        },
        mounted() {
            if (localStorage.getItem('password')) {
                this.password = localStorage.getItem('password');
            }
        }
    }
</script>