<template>
    <section>
        <button class="button is-info"
            @click="isComponentModalActive = true">
            <span class="icon">
                    <i class="fa fa-location-arrow"></i>
                </span>
            <span>Login</span>
        </button>

        <b-modal :active.sync="isComponentModalActive" has-modal-card>
           
                <div class="modal-card" style="min-width: 480px;">
                    <header class="modal-card-head modal-card-head-login">
                        <p class="modal-card-title">Login</p>
                    </header>
                    <section class="modal-card-body">
                       <div class="field">
                           <label class="label">Email</label>
                            <p class="control has-icons-left has-icons-right">
                                <input class="input" type="email" placeholder="E.g. mike@test.com" v-model="user.email" required>
                                <span class="icon is-small is-left">
                                <i class="fas fa-envelope"></i>
                                </span>
                            </p>
                        </div>

                        <div class="field no-margin-left">
                            <label class="label">Password</label>
                            <!-- <b-input
                                type="password"
                                v-model = "user.password"
                                password-reveal
                                placeholder="Your password"
                                required
                                @keydown.native.enter="login()">
                            </b-input> -->
                            
                            <p class="control has-icons-left has-icons-right">
                                <input class="input" type="password" placeholder="E.g. password" v-model="user.password" @keydown.native.enter="login()" required>
                                <span class="icon is-small is-left">
                                <i class="fas fa-key"></i>
                                </span>
                            </p>

                        </div>

                        <div class="field no-margin-left">
                            <span class="message"  v-if="error.length">{{error}}</span>
                             <span  v-else>&nbsp;</span>
                        </div>
                    </section>
                    <footer class="modal-card-foot">
                        <button class="button" type="button" @click="isComponentModalActive = false">Close</button>
                        <button class="button is-info" @click="login">Login</button>
                    </footer>
                </div>
        </b-modal>
    </section>
</template>

<script>
    export default {
        data() {
            return {
                isComponentModalActive: false,
                error:"",
                user: {
                    email: 'mike@test.com',
                    password: 'password'
                },
            }
        },
        methods: {
            login() {
               
                if (this.user.password == "" || this.user.email == "") {
                    this.error = "Invalid Username and Password";
                }
                else {
                    axios.post('login', {email:this.user.email, password:this.user.password})
                    .then((response)=>{
                            this.isComponentModalActive = false;
                            this.$parent.userLogin = true;
                            window.location = "/dashboard";
                            
                    })
                    .catch((error) => {
                        this.error = (error.response.data.email).replace(/[^a-zA-Z ]/g, "");
                });
                }
     

                 
            },
        }
    }
</script>