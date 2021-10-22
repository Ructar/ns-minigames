<template>
    <div class="row items-center justify-center" style="background-color: white; width: 100%">
        <div class="col-8" style="height:250px">
            <q-card class="bg-blue-grey-10" square style="height: 100%">
                <q-card-section>
                    <transition-group appear enter-active-class="animated fadeIn" leave-active-class="animated fadeOut">
                        <div class="row justify-evenly" style="width: 100%">
                            <div class="col-2 triangle" v-show="showTriangle" v-for="triangle in triangles" :key="triangle">
                                <p class="triangle-left">{{triangle.left}}</p>
                                <p class="triangle-top">{{triangle.top}}</p>
                                <p class="triangle-right">{{triangle.right}}</p>
                                <p class="triangle-center" v-if="triangle.center !== 0">{{triangle.center}}</p>
                            </div>
                        </div>
                        <q-form @submit="submitAnswer">
                            <q-input dense name="answer" v-model="answer"></q-input>
                        </q-form>
                    </transition-group>
                </q-card-section>
                <q-inner-loading :showing="loading">
                    <p>Human Captcha Required</p>
                    <q-spinner-gears size="50px" color="primary" />
                </q-inner-loading>
            </q-card>
        </div>
    </div>
</template>

<script>
    import { ref } from "vue"
    export default { 
       name: "Triangle",
       data() {
           const loading = ref(false)
           const showTriangle = ref(false)

            return {
                loading, showTriangle,
                maxInterval: 8,
                defaultTriangles: [
                    { top: 0, left: 0, center: 0, right: 0 },
                    { top: 0, left: 0, center: 0, right: 0 },
                    { top: 0, left: 0, center: 0, right: 0 },
                    { top: 0, left: 0, center: 0, right: 0 },
                ],
                pattern: [],
                showTriangleLoading () {
                    loading.value = true
                    showTriangle.value = false

                    setTimeout(() => {
                        loading.value = false
                        showTriangle.value = true
                    }, 100)
                }
            }
       },
       methods: {

       },
       computed: {
           triangles() {
               return this.defaultTriangles
           }
       },
       created() {
           this.defaultTriangles.forEach(triangle => {
                triangle.top = Math.floor( Math.random() * ( this.maxInterval ) )
                triangle.left = Math.floor( Math.random() * ( triangle.top - 1) )
                triangle.right = Math.floor( Math.random() * ( this.maxInterval ) )
                triangle.center = ( triangle.top - triangle.left ) * triangle.right
                if ( triangle.top < 1 ) {
                    triangle.top = 2
                }
                if ( triangle.left < 1) {
                    triangle.left = 2
                }
                if ( triangle.right < 1 ) {
                    triangle.right = 2
                }
           });
           this.defaultTriangles[3].center = 0
           this.showTriangleLoading();
       }
    }
</script>

<style scoped>
    .triangle {
        width: 0 !important;
        border-left: 80px solid transparent;
        border-right: 80px solid transparent;
        border-bottom: 160px solid #555;
        font-size: 40px;
    }

    .triangle-top { 
        position: absolute;
        margin-left: -10px;
        margin-top: -15px;
    }

    .triangle-left {
        position: absolute;
        margin-left: -93px;
        padding-top: 130px;
    }

    .triangle-right {
        position: absolute;
        margin-left: 70px;
        padding-top: 130px;
    }

    .triangle-center {
        position: absolute;
        margin-left: -10px;
        padding-top: 70px;
    }
</style>