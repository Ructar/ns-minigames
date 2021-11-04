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
                        <q-form style="margin-top: 10px;" @submit="submitAnswer">
                            <q-input dense name="answer" v-model="answer"></q-input>
                        </q-form>
                        <div class="text-center" style = "padding-top: 4px">
                            <p v-if="correct === true" style="color: green">Success</p>
                            <p v-if="correct === false" style="color: red">Failure</p>
                        </div>
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
                },
                correctAnswer: null,
                answer: null,
                correct: null,
            }
       },
       methods: {
           submitAnswer() {
               if ( this.answer === this.correctAnswer ) { 
                   this.correct = true
                   console.log("True")
               } else {
                   this.correct = false
                   console.log("False: " + this.correctAnswer)
               }
           },
           randomInt(min, max) {
               return Math.floor(Math.random() * (max - min + 1) + min)
           }
       },
       computed: {
           triangles() {
               return this.defaultTriangles
           }
       },
       created() {
           this.defaultTriangles.forEach(triangle => {
                triangle.top = this.randomInt(6, 10)
                triangle.left = this.randomInt(1, 5)
                triangle.right = this.randomInt(2, 4)
                triangle.center = ( triangle.top - triangle.left ) * triangle.right
           });
           this.correctAnswer = this.defaultTriangles[3].center.toString()
           this.defaultTriangles[3].center = "?"
           this.showTriangleLoading();
           setTimeout(() => {
               if ( this.correct ) {
                   this.correct = true
               } else {
                   this.correct = false
               }
           }, 5000)
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