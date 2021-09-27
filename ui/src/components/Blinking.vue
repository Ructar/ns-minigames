<template>
    <div class="row items-center justify-center" style="background-color: white; width: 100%">
        <div class="col-3" style="height: 550px">
            <q-card class="bg-blue-grey-10" square style="height: 100%">
                <div class="q-gutter-xs row justify-center grid-container" v-if="toggleGrid === true">
                    <div class="box" v-for="square in squares" :key="square" :class="square.class" v-on:click="renderClick(square.value)" />
                </div>
                <p class="text-center text-white" v-if="toggleGrid === true">Match</p>
                <p class="text-center text-white" v-else>Incorrect</p>
            </q-card>
        </div>
    </div>
</template>

<script>
export default {
    name: "blinking",
    data () {
        return{ 
            toggleGrid: true,
            incorrect: 0,
            maxIncorrect: 2,
            pattern: [],
            numberOfSquares: 25,
            defaultSquares: [
                { value: 0, class: "bg-grey-6" },
                { value: 1, class: "bg-grey-7" },
                { value: 2, class: "bg-grey-6" },
                { value: 3, class: "bg-grey-7" },
                { value: 4, class: "bg-grey-6" },
                { value: 5, class: "bg-grey-7" },
                { value: 6, class: "bg-grey-6" },
                { value: 7, class: "bg-grey-7" },
                { value: 8, class: "bg-grey-6" },
                { value: 9, class: "bg-grey-7" },
                { value: 10, class: "bg-grey-6" },
                { value: 11, class: "bg-grey-7" },
                { value: 12, class: "bg-grey-6" },
                { value: 13, class: "bg-grey-7" },
                { value: 14, class: "bg-grey-6" },
                { value: 15, class: "bg-grey-7" },
                { value: 16, class: "bg-grey-6" },
                { value: 17, class: "bg-grey-7" },
                { value: 18, class: "bg-grey-6" },
                { value: 19, class: "bg-grey-7" },
                { value: 20, class: "bg-grey-6" },
                { value: 21, class: "bg-grey-7" },
                { value: 22, class: "bg-grey-6" },
                { value: 23, class: "bg-grey-7" },
                { value: 24, class: "bg-grey-6" },
            ]
        }
    },
    methods: {
        setPattern(i, previousNumber, previousClass) {
            setTimeout(() => {
                if ( previousNumber != null && previousClass != null ) {
                        this.defaultSquares[previousNumber].class = previousClass
                    }
                if ( i != 1 ) {
                    let randomNumber = Math.floor(Math.random() * 23)
                    this.pattern.push(randomNumber)
                    previousNumber = randomNumber
                    previousClass = this.defaultSquares[previousNumber].class
                    this.defaultSquares[randomNumber].class = "bg-blue-4"
                    if ( --i ) this.setPattern(i, previousNumber, previousClass)
                }
            }, 400);
        },
        renderClick(value) {
            if ( value === this.pattern[0]) {
                this.pattern.shift();
                this.defaultSquares[value].class = "bg-green-4"
                if( this.pattern.length <= 0 ) {
                    console.log("You won!")
                }
            } else {
                this.incorrect = this.incorrect + 1
                this.defaultSquares[value].class = "bg-red-4"
                if ( this.incorrect >= this.maxIncorrect) {
                    this.toggleGrid = false
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
        this.setPattern(9)
    }
}
</script>

<style scoped>
    .container {
        height: 100%;
        width: 100%;
    }

    .box {
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