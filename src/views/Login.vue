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
        <button type="button" v-on:click="login()" v-if="loading === false">Login</button>
        <div class="lds-dual-ring" v-if="loading"></div>
        <div class="error-message" v-if="showError">{{ error }}</div>
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
                },
                loading: false,
                showError: false,
                error: ""
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
                this.loading = true;
                let port = this.$parent.backendServerPort;

                $.ajax({
                    url: 'http://localhost:'+port+'/api/v1/auth/login',
                    type: 'POST',
                    data: JSON.stringify(loginData),
                    crossDomain: true,
                    dataType: 'json',
                    contentType: 'application/json',
                    success: function(data) {
                        _this.loading = false;
                        _this.$emit('authenticated', {
                            status: true, 
                            token: data.token, 
                            user: data.user
                        });
                        _this.$router.replace({ name: 'Secure' });
                    },
                    error: function(xhr){
                        _this.loading = false;
                        _this.error = xhr.responseJSON.title;
                        _this.showError = true;
                        console.log(xhr);
                        setTimeout(function() {
                            _this.showError = false;
                        }, 1500);
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

.error-message {
    padding: 20px;
    background-color: #f44336; /* Red */
    color: white;
    margin-bottom: 15px;
    margin-top: 20px;
    max-width: 200px;
    margin: 0 auto
}

/* loader */

.lds-dual-ring {
  display: inline-block;
  width: 80px;
  height: 80px;
}
.lds-dual-ring:after {
  content: " ";
  display: block;
  width: 64px;
  height: 64px;
  margin: 8px;
  border-radius: 50%;
  border: 6px solid rgb(71, 174, 20);
  border-color: rgb(71, 174, 20) transparent rgb(71, 174, 20) transparent;
  animation: lds-dual-ring 1.2s linear infinite;
}
@keyframes lds-dual-ring {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

</style>