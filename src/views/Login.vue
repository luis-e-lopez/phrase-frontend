<template>
    <div id="login">
        <h1>Login</h1>
        <div class="form-inputs">
            <label for="username">Username</label>
            <input type="text" id="username" name="username" v-model="input.username" placeholder="Username" />
        </div>
        <div class="form-inputs">
            <label for="password">Password</label>
            <input type="password" id="password" name="password" v-model="input.password" placeholder="Password" />
        </div>
        <button type="button" v-on:click="login()">Login</button>
    </div>
</template>

<script>
    import $ from "jquery";

    export default {
        name: 'Login',
        data() {
            return {
                input: {
                    username: "",
                    password: ""
                }
            }
        },
        methods: {
            login() {
                if(this.input.username != "" && this.input.password != "") {
                    this.authenticateUser();
                } else {
                    console.log("A username and password must be present");
                }
            },
            authenticateUser() {
                let loginData = { 'userName': this.input.username, 'password': this.input.password };
                let _this = this;
                $.ajax({
                    url: 'http://localhost:8081/api/v1/auth/login',
                    type: 'POST',
                    data: JSON.stringify(loginData),
                    crossDomain: true,
                    dataType: 'json',
                    contentType: 'application/json',
                    success: function(data) {
                        console.log(data);
                        _this.$emit('authenticated', {
                            status: true, 
                            token: data.token, 
                            user: data.user
                        });
                        _this.$router.replace({ name: 'Secure' });
                    },
                    error: function(xhr){
                        console.log(xhr);
                    }
                });
            }
        }
    }
</script>

<style>

#login .form-inputs {
    padding-bottom: 10px;
}

#login .form-inputs label {
    padding-right: 10px;
}

</style>