<template>
    <div id="createUser">
        <div id="pop" class="container-flex mx-4   mt-5 fixed-top">
            <div class="row justify-content-center">
                <div class="card align-item-center d-flex "
                    style="width:80%;height: 80vh;position: absolute; z-index:10">
                    <div class="card-header d-flex justify-content-between">
                        <h4>Create User</h4>
                        <button class="btn btn-warning" @click="cancel()">Cancel</button>
                    </div>
                    <div class="card-body overflow-auto">
                        <div class="row mt-3 justify-content-center" v-if="info === true">
                            <span class="text-danger" v-if="msg.general"
                                style="background-color:#f4f4f4e3;">{{ msg.general }}</span>
                        </div>
                        <div class="row mt-2 justify-content-center">
                            <label>firstName</label>
                        </div>
                        <div class="row mt-2 justify-content-center">
                            <input type="text" name="name" required placeholder="firstName" v-model="firstName">
                        </div>
                        <div class="row mt-3 justify-content-center">
                            <span class="text-danger" v-if="msg.firstName"
                                style="background-color:#f4f4f4e3;">{{msg.firstName}}</span>
                        </div>
                        <div class="row mt-2 justify-content-center">
                            <label>LastName</label>
                        </div>
                        <div class="row mt-2 justify-content-center">
                            <input type="text" name="name" required placeholder="LastName" v-model="lastName">
                        </div>
                        <div class="row mt-3 justify-content-center">
                            <span class="text-danger" v-if="msg.lastName"
                                style="background-color:#f4f4f4e3;">{{msg.lastName}}</span>
                        </div>
                        <div class="row mt-2 justify-content-center">
                            <label>Phone</label>
                        </div>
                        <div class="row mt-2 justify-content-center">
                            <input type="text" name="phone" required placeholder="phone" v-model="phone">
                        </div>
                        <div class="row mt-3 justify-content-center">
                            <span class="text-danger" v-if="msg.phone"
                                style="background-color:#f4f4f4e3;">{{msg.phone}}</span>
                        </div>
                        <div class="row mt-2 justify-content-center">
                            <label>Mail Adress</label>
                        </div>
                        <div class="row mt-2 justify-content-center">
                            <input type="text" name="email" required placeholder="email" v-model="email">
                        </div>
                        <div class="row mt-3 justify-content-center">
                            <span class="text-danger" v-if="msg.email"
                                style="background-color:#f4f4f4e3;">{{msg.email}}</span>
                        </div>
                        <div class="row mt-2 justify-content-center">
                            <label>Password</label>
                        </div>
                        <div class="row mt-2 justify-content-center">
                            <input type="password" name="password" required v-model="password">
                        </div>
                        <div class="row mt-3 justify-content-center">
                            <span class="text-danger" v-if="msg.password"
                                style="background-color:#f4f4f4e3;">{{msg.password}}</span>
                        </div>
                        <div class="row mt-2 justify-content-center">
                            <label>Password Confirm</label>
                        </div>
                        <div class="row mt-2 justify-content-center">
                            <input type="password" name="passwordComfirme" required v-model="passwordComfirme">
                        </div>
                        <div class="row mt-3 justify-content-center">
                            <span class="text-danger" v-if="msg.passwordComfirme"
                                style="background-color:#f4f4f4e3;">{{msg.passwordComfirme}}</span>
                        </div>
                        <div class="row mt-2 justify-content-center">
                            <label>Admin ?</label>
                        </div>
                        <div class="row mt-2 justify-content-center">
                            <input type="checkbox" name="isAdmin" v-model="isAdmin">
                        </div>
                    </div>
                    <button class="btn btn-success p-4" style="width:100%" @click="submit()">Create</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import ENV from "../../env.config";
    /**
     * Pop Up window for create user
     * @displayName createUser
     */
    export default {
        name: 'createUser',
        data() {
            return {
                firstName: "",
                lastName: "",
                phone: "",
                email: "",
                password: "",
                passwordComfirme: "",
                isAdmin: false,
                msg: [],
                validator: [],
                info: false,
            }
        },
        watch: {
            email(value) {
                this.email = value;
                this.validateEmail(value);
            },
            firstName(value) {
                this.firstName = value;
                this.validateFirstName(value);
            },
            lastName(value) {
                this.lastName = value;
                this.validateLastName(value);
            },
            phone(value) {
                this.phone = value;
                this.validatePhone(value);
            },
            password(value) {
                this.password = value;
                this.validatePassword(value);
            },
            passwordComfirme(value) {
                this.passwordComfirme = value;
                this.validatePasswordComfirme(value);
            },
        },
        async mounted() {
            this.validator['email'] = false;
            this.validator['firstName'] = false;
            this.validator['lastName'] = false;
            this.validator['phone'] = false;
            this.validator['password'] = false;
        },
        methods: {
            /**
             * Check if email is valid with regex
             * @param {string} value
             * @public
             */
            validateEmail(value) {
                if (/^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/.test(value)) {
                    this.msg['email'] = '';
                    this.validator['email'] = true;
                } else {
                    this.msg['email'] = 'Incorrect email address';
                    this.validator['email'] = false;
                }
            },
            /**
             * Check if firstname is valid with regex
             * @param {string} value
             * @public
             */
            validateFirstName(value) {
                if (/^[a-zA-Z0-9_]{1,16}$/.test(value)) {
                    this.msg['firstName'] = '';
                    this.validator['firstName'] = true;
                } else {
                    this.msg['firstName'] = 'Your fisrtname must contain between 1 and 16 characters';
                    this.validator['firstName'] = false;
                }
            },
            /**
             * Check if lastname is valid with regex
             * @param {string} value
             * @public
             */
            validateLastName(value) {
                if (/^[a-zA-Z0-9_]{1,16}$/.test(value)) {
                    this.msg['lastName'] = '';
                    this.validator['lastName'] = true;
                } else {
                    this.msg['lastName'] = 'Your lastname must contain between 1 and 16 characters';
                    this.validator['lastName'] = false;
                }
            },
            /**
             * Check if phone is valid with regex
             * @param {string} value
             * @public
             */
            validatePhone(value) {
                if (/^(\d\d\s){4}(\d\d)$/.test(value)) {
                    this.msg['phone'] = '';
                    this.validator['phone'] = true;
                } else {
                    this.msg['phone'] = 'Your number must be like 00 00 00 00 00';
                    this.validator['phone'] = false;
                }
            },
            /**
             * Check if password is valid with regex
             * @param {string} value
             * @public
             */
            validatePassword(value) {
                if (/^(?=.*[a-z])(?=.*[A-Z])(?=.*[0-9])(?=.*\W).{8,}$/.test(value)) {
                    this.msg['password'] = '';
                } else {
                    this.msg['password'] =
                        'Your password must have a minimum of 8 characters (1 uppercase, 1 lowercase, 1 number, 1 special character minimum)';
                }
            },
            /**
             * Check if password comfirmation is equal to password
             * @param {string} value
             * @public
             */
            validatePasswordComfirme(value) {
                if (this.password === this.passwordComfirme) {
                    this.msg['passwordComfirme'] = '';
                    this.validator['password'] = true;
                } else {
                    this.msg['passwordComfirme'] = 'Comfirmation dosen\'t match';
                    this.validator['password'] = false;
                }
            },
            /**
             * After all control in form create new user
             *
             * @public
             */
            async submit() {
                if (this.validator.firstName === true && this.validator.lastName === true && this.validator
                    .email === true && this.validator.phone && this.validator.password ===
                    true) {
                    let body = {
                        firstName: this.firstName,
                        lastName: this.lastName,
                        email: this.email,
                        phone: this.phone,
                        password: this.password,
                        isAdmin: this.isAdmin
                    }
                    let requestStatus;
                    await fetch(`http://${ENV.API_BACKEND}/users/admin/createUser`, {
                            "method": "POST",
                            "headers": {
                                "content-type": "application/json",
                                "authorization": "Bearer " + this.$store.state.access_token
                            },
                            "body": JSON.stringify(body)
                        })
                        .then(res => {
                            requestStatus = res.status
                        })
                    if (requestStatus === 400) {
                        this.info = true;
                        this.msg['general'] = "Account existing for " + this.email;
                    } else {
                        let list = [];
                        await fetch(`http://${ENV.API_BACKEND}/users`, {
                                "method": "GET",
                                "headers": {
                                    "authorization": "Bearer " + this.$store.state.access_token
                                }
                            })
                            .then(res => res.clone().json())
                            .then(json => list = json);
                        this.$parent.listUser = list;
                        this.$parent.createButton = false;
                    }
                } else {
                    this.info = true;
                    this.msg['general'] = "Information missing in the form";
                }
            },
            /**
             * Close this Pop Up
             *
             * @public
             */
            cancel() {
                this.$parent.createButton = false;
            }
        }
    }
</script>

<style scoped>
    @import url('https://fonts.googleapis.com/css2?family=Roboto+Mono&display=swap');

  .card-header {
    font-family: Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
    text-shadow: 0px 0px 10px rgba(0, 0, 0, 0.91);
    color: #ffffff !important;
    border-bottom: 3px solid rgb(255, 255, 255);
    background: rgb(131, 58, 180);
    background: linear-gradient(90deg, rgba(194, 6, 62, 0.527) 0%, rgba(6, 188, 194, 0.527) 50%, rgb(69, 252, 124) 100%);
  }

  .card-body {
    background: rgb(174, 238, 230);
    background: -moz-radial-gradient(circle, rgba(174, 238, 230, 1) 17%, rgba(31, 121, 236, 1) 100%);
    background: -webkit-radial-gradient(circle, rgba(174, 238, 230, 1) 17%, rgba(31, 121, 236, 1) 100%);
    background: radial-gradient(circle, rgba(174, 238, 230, 1) 17%, rgba(31, 121, 236, 1) 100%);
    filter: progid:DXImageTransform.Microsoft.gradient(startColorstr="#aeeee6", endColorstr="#1f79ec", GradientType=1);
  }
</style>