<template>
    <div>
        <Hero :message="hero.message" :title="hero.title"></Hero>

        <!--{{admin}} <br>-->
        <!--{{adminData}}-->
        <section class="section">
            <div class="columns is-mobile">
                <div class="column is-half is-offset-one-quarter">
                    <h1 class="title" style="text-align: left">Compliance Log In</h1>
                    <b-field label="Compliance Manager Id">
                        <b-input v-model='admin.id' maxlength="30" required></b-input>
                    </b-field>

                    <b-field label="Password">
                        <b-input type="password"
                                 v-model="admin.password"
                                 password-reveal
                        required>
                        </b-input>
                    </b-field>
                </div>
            </div>
            <div class="columns is-mobile">
                <div class="column is-half is-offset-8">
                    <button class="button is-info is-right" slot="trigger" @click="logIn">Log In</button>

                </div>
            </div>
        </section>
    </div></template>
<script>
    import Hero from '../Hero';

    export default {
        components:{
            Hero
        },
        data() {
            return {
                hero:{
                    title:"Welcome to DB Compliance",
                    message:"Please Login for Managing issues...",
                },
                admin: {} //we will use this temperary varible and use computed property for rendering data.

            }
        },
        methods: {
            logIn() {
                console.log("admin trying to login....");
                this.$store.dispatch('adminLogIn', this.admin).then(response=>{
                    if(response.code==1) {
                        notification(this, "Success! logged in..");
                        this.$router.push("/compliance/dashboard");
                    }else if(response.code==0) {
                        notification(this, "Error! Compliance Manager not found, try again..");
                    }else{
                        notification(this, "Error! Wrong password for Compliance Id..");
                    }
                });
            }
        },
        computed: { //we will use computed properties to get admindata from state.
            adminData() {
                return this.$store.getters.getAdmin;
            }
        },

        //created() and mounted() both are being called everytime we change route to this url
        mounted() {
            // console.log("mounted");
            // console.log(this.$store.getters.isLoggedIn);
            if (this.$store.getters.isAdminLoggedIn) {
                console.log("already logged in");
                this.$router.push("/compliance/dashboard");
                //if loggedIn, then update our admin object with state
                //and go to dashboard
            } else {
                notification(this,'Please Login as Compliance Manager to continue.');
            }
        }
    }
</script>

<style scoped>
    .section{
        text-align: left;
    }

</style>