<template>
    <div class="row items-center justify-center" style="background-color: white; width: 100%">
        <div class="col-3" style="height: 550px">
            <q-card class="bg-blue-grey-10" square style="height: 100%">
                <q-card-section v-show="!showGrid">
                    <div class="text-center" v-if="!failed">
                        <h4>Human Captcha Required</h4>
                        <h5>Match the following pattern in order</h5>
                        <q-icon name="fingerprint" size="200px"/>
                    </div>
                    <div class="text-center" v-else>
                        <h4 class="text-red-8">Invalid Captcha Entered</h4>
                        <q-icon name="fingerprint" size="200px" color="red"/>
                    </div>
                </q-card-section>
                <q-card-section>
                    <transition-group
                        appear
                        enter-active-class="animated fadeIn"
                        leave-active-class="animated fadeOut"
                        >
                        <div class="q-gutter-xs row justify-center grid-container" v-show="showGrid">
                            <div class="box" v-for="square in squares" :key="square" :class="square.class" v-on:click="renderClick(square.value)" />
                        </div>
                        <p class="text-center text-white" v-show="showGrid">Match</p>
                    </transition-group>
                </q-card-section>
                <q-inner-loading :showing="loading">
                    <p>Starting Hackersman Stuff</p>
                    <q-spinner-gears size="50px" color="primary" />
                </q-inner-loading>
            </q-card>
        </div>
    </div>
</template>

<script>
import { ref } from "vue"
export default {
    name: "blinking",
    data () {
        const loading = ref(false)
        const showGrid = ref(false)
        return {
            failed: false,
            loading, showGrid,
            toggleGrid: true,
            canClick: false,
            incorrect: 0,
            maxIncorrect: 2,
            patternSize: 8,
            pattern: [],
            numberOfSquares: 25,
            defaultSquares: [],
            showGridLoading () {
                loading.value = true
                showGrid.value = false
                setTimeout(() => {
                    loading.value = false
                    showGrid.value = true
                    this.setPattern()
                }, 10000)
            }
        }
    },
    methods: {
        generateBoxes() {
            for ( let i = 0; i < this.numberOfSquares; i++ ) {
                if ( i % 2 === 0 ) {
                    this.defaultSquares[i] = { value: i, class: "bg-grey-6"}
                } else {
                    this.defaultSquares[i] = { value: i, class: "bg-grey-7"}
                }
            }
        },
        setPattern() {
            for ( let i = 0; i < this.patternSize; i++ ) {
                let randomNumber = Math.floor( Math.random() * ( this.numberOfSquares - 1 ) )
                this.pattern.push(randomNumber)
            }
            this.displayPattern(0, null)
        },
        displayPattern(i, previousNumber, previousClass) {
            setTimeout(() => {
                let index = this.pattern[i]
                if ( previousClass != null ) { 
                    this.defaultSquares[previousNumber].class = previousClass
                }
                if ( i != this.pattern.length ) {
                    previousClass = this.defaultSquares[index].class
                    this.defaultSquares[index].class = "bg-blue-4"
                    if ( ++i && i < this.pattern.length + 1) this.displayPattern(i, index, previousClass)
                } else {
                    this.canClick = true
                }
            }, 800);
        },
        renderClick(value) {
            if ( this.canClick ) {
                if ( value === this.pattern[0]) {
                    this.pattern.shift();
                    let previous = this.defaultSquares[value].class
                    this.defaultSquares[value].class = "bg-green-4"
                    setTimeout(() => {
                        this.defaultSquares[value].class = previous
                    }, 1000)
                    if( this.pattern.length <= 0 ) {
                        console.log("You won!")
                        this.canClick = false
                    }
                } else {
                    this.incorrect = this.incorrect + 1
                    this.defaultSquares[value].class = "bg-red-4"
                    if ( this.incorrect >= this.maxIncorrect) {
                        this.showGrid = false
                        this.failed = true
                        this.canClick = false
                    }
                }
            }
        }
    },
    computed: {
        squares() {
            return this.defaultSquares
        }
    },
    created() {
        this.generateBoxes()
        this.showGridLoading()
    }
}
</script>

<style scoped>
    .container {
        height: 100%;
        width: 100%;
    }

    .box {
        transition: 5ms;
        height: 75px;
        width: 75px;
    }

    .grid-container {
        padding-top: 50px;
        padding-bottom: 50px;
        padding-right: 25px;
        padding-left: 25px;    
    }

</style>