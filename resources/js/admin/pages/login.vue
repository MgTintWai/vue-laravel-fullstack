<template>
    <div>
        <div class="container">
            <div
                class="_1adminOverveiw_table_recent _box_shadow _border_radious _mar_b30 _p20 col-md-4"
            >
                <div class="login_header">
                    <h1>Login to Dashboard</h1>
                </div>
                <div class="space">
                    <Input
                        type="email"
                        v-model="data.email"
                        placeholder="Email"
                    />
                </div>
                <div class="space">
                    <Input
                        type="password"
                        v-model="data.password"
                        placeholder="Password"
                    />
                </div>
                <div class="login_footer">
                    <Button
                        type="primary"
                        @click="login"
                        :disabled="isLoggedIn"
                        :loading="isLoggedIn"
                        >{{ isLoggedIn ? "Logging..." : "Login" }}</Button
                    >
                </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    data() {
        return {
            data: {
                email: "",
                password: "",
            },
            isLoggedIn: false,
        };
    },

    methods: {
        async login() {
            if (this.data.email.trim() == "")
                return this.e("Email is required!");
            if (this.data.password.trim() == "")
                return this.e("Password is required!");
            if (this.data.password.length < 6)
                return this.e("Incorrect Login details");
            this.isLoggedIn = true;
            const res = await this.callApi(
                "post",
                "app/admin_login",
                this.data
            );
            if (res.status === 200) {
                this.s(res.data.msg);
                window.location = '/'
            } else {
                if (res.status == 401) {
                    this.i(res.data.msg);
                } else if (res.status == 422) {
                    for (let i in res.errors) {
                        this.w(res.errors[i][0]);
                    }
                } else {
                    this.swr();
                }
            }
            this.isLoggedIn = false;
        },
    },
};
</script>

<style scoped>
._1adminOverveiw_table_recent {
    margin: 0 auto;
    margin-top: 150px;
}

.login_footer {
    text-align: center;
}
.login_header {
    text-align: center;
    margin-bottom: 25px;
}
</style>
