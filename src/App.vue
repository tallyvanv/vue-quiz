<template>
    <div id="app">
        <Header
                :numCorrect="numCorrect"
                :numTotal="numTotal"/>
        <b-container class="bv-example-row">
            <b-row>
                <b-col sm="6" offset="3">
                    <QuestionBox
                            v-if="questions.length"
                            :currentQuestion="questions[index]"
                            :next="next"
                            :increment="increment"
                    />
                </b-col>
                <b-modal ref="disc" title="Quiz over!" size="lg" body-bg-variant="dark" header-bg-variant="dark"
                         header-text-variant="light" body-text-variant="light" hide-footer title-class="text-light">
                    <div class="text-justify modal-text">
                        <p>Your score is: {{ numCorrect }}/{{ numTotal }}</p>
                    </div>
                    <b-button @click="reloadPage">Start again</b-button>
                </b-modal>
            </b-row>
        </b-container>
    </div>
</template>

<script>
    import Header from './components/Header.vue'
    import QuestionBox from './components/QuestionBox.vue'

    export default {
        name: 'App',
        components: {
            Header,
            QuestionBox
        },
        data() {
            return {
                questions: [],
                index: 0,
                numCorrect: 0,
                numTotal: 0
            }
        },
        methods: {
            next() {
                this.index++
                if (this.index === 10) {
                    this.$refs['disc'].show()
                }
            },
            increment(isCorrect) {
                if (isCorrect) {
                    this.numCorrect++
                }
                this.numTotal++
            },
            reloadPage() {
                window.location.reload()
            }

        },
        mounted: function () {
            fetch('https://opentdb.com/api.php?amount=10&category=12&type=multiple', {
                method: 'get'
            })
                .then((response) => {
                    return response.json()
                })
                .then((jsonData) => {
                    this.questions = jsonData.results
                })
        }
    }
</script>

<style>
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
</style>
