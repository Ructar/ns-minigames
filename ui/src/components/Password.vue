<template>
    <div class="row items-center justify-center" style="background-color: white; width: 100%">
        <div class="col-3" style="height:200px">
            <q-card class="bg-blue-grey-10" square style="height: 100%">
                <q-card-section>
                    <transition-group appear enter-active-class="animated fadeIn" leave-active-class="animated fadeOut">
                        <div class="text-center" v-show="showPassword">
                            <q-form @submit="submitPassword">
                                <q-icon name="fingerprint" size="50px" style="padding-bottom: 20px"/>
                                <p>Enter password</p>
                                <q-input dense name="password" v-model="password"></q-input>
                            </q-form>
                            <div style="padding-top: 10px">
                                <p v-if="success" style="color: green">Success</p>
                                <p v-if="failure" style="color: red">Failure</p>
                            </div>
                        </div>
                    </transition-group>
                </q-card-section>
                <q-inner-loading :showing="loading">
                    <p>Authentication Required</p>
                    <q-spinner-gears size="50px" color="primary" />
                </q-inner-loading>
            </q-card>
        </div>
    </div>
</template>

<script>
    import { ref } from "vue"

    export default {
        name: "password",
        data () {
            const loading = ref(false)
            const showPassword = ref(false)

            return { 
                loading, showPassword,
                password: "",
                success: false, 
                failure: false,
                secret: "allentate",
                showPasswordLoading () {
                    loading.value = true
                    showPassword.value = false

                    setTimeout(() => {
                        loading.value = false
                        showPassword.value = true
                    }, 5000)
                }
            }
        },
        methods: {
            submitPassword () {
                    if ( this.password === this.secret) {
                        this.failure = false
                        this.success = true
                    } else {
                        this.success = false
                        this.failure = true
                    }

                },
        },
        computed: {

        },
        created() {
            this.showPasswordLoading()
        }
    }
</script>

<style scoped>

</style>