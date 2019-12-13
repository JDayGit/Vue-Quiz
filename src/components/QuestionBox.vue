<template>
        <div id="question-box-container">
            <b-jumbotron>
            <!-- QUESTION -->
                <template slot="lead">
                    {{ currentQuestion.question }}
                </template>

                <hr class="my-4" />
            <!-- ANSWERS -->
                <b-list-group id="answer-group">
                    <b-list-group-item 
                        v-for="(answer, index) in shuffledAnswers" 
                        :key="index"
                        @click.prevent="selectAnswer(index)"
                        :class="answerClass(index)"
                        >
                            {{ answer }}
                    </b-list-group-item>
                </b-list-group>

                <b-button 
                    variant="primary"
                    @click="submitAnswer"
                    :disabled="selectedIndex === null || answered"
                >
                    Submit
                </b-button>
                <b-button @click="next" variant="success">Next</b-button>

            </b-jumbotron>
         </div>
</template>

<script>
import _ from 'lodash'
export default {
    name: "QuestionBox",
    props: {
        currentQuestion: Object,
        next: Function,
        increment: Function,
    },
    data: function () {
        return {
            selectedIndex: null,
            correctIndex: null,
            shuffledAnswers: [],
            answered: false
        }
    },
    computed: {
        answers() {
            // this function is no longer used in finished code
            // it is replaced by the watch function below and the
            // shuffleAnswers method
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            return answers
        },
    },
    watch: {
        currentQuestion: {
            immediate: true,
            handler() {
                this.selectedIndex = null
                this.answered = false
                this.shuffleAnswers()
            }
        }
    },
    methods: {
        selectAnswer(index) {
            this.selectedIndex = index
        },
        submitAnswer() {
            let isCorrect = false    
            if (this.selectedIndex === this.correctIndex) {
                isCorrect = true
            }
            this.answered = true
            this.increment(isCorrect)
        },
        shuffleAnswers() {
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffledAnswers = _.shuffle(answers)
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
        },
        answerClass(index) {
            let answerClass = ''
            if (!this.answered && this.selectedIndex === index) {
                answerClass = 'selected'
            } else if (this.answered && this.correctIndex === index) {
                answerClass = 'correct'
            } else if (this.answered && this.selectedIndex === index && this.correctIndex !== index) {
                answerClass = 'incorrect'
            }
            return answerClass
        }
    }
}
</script>


<style scoped>
/* QUESTION NUMBER */
#questionNumber {
    font-size:20px;
    font-weight:lighter;
    margin-top: -4vh;
    margin-bottom: 4vh;
    text-align: left;
}
/* FULL COMPONENT CONTAINER */
#question-box-container {
    text-align:center;
    margin-top: 5vh;
}

/* QUESTION PARAGRAPH TAG */
.lead {
    font-size:26px;
    font-weight:500;
    color: #2c3e50;
}

/* SUBMIT & NEXT BUTTONS */
.btn {
    margin: 6px;
    width: 120px;
    font-size:19px;
    margin-bottom: -4vh;
    font-weight:500;
    padding: 8px;
}

.btn:hover {
    font-weight: bolder;
}

/* ANSWERS CONTAINER */
#answer-group {
    margin-bottom: 3vh;
}

.list-group-item {
    font-size: 20px;
    padding: 20px;
    color: #2c3e50;
    border-width: 2px;
}

/* INDIVIDUAL ANSWERS */
.list-group-item:hover {
    cursor: pointer;
    font-weight: 600;
    font-size:24px;
    background: #2c3e50;
    color: white;
}

/* CONDITIONAL STYLING BASED ON ANSWER */
.selected {
    color: white;
    background: #007bff;
    font-weight: bolder;
}

.selected:hover {
    color: white;
    background: #007bff;
    font-weight: bolder; 
}

.correct {
    background: #5cb85c;
    color: white;
    font-weight: bolder;
}

.correct:hover {
    background: #5cb85c;
    color: white;
    font-weight: bolder;
}

.incorrect {
    background: #d9534f;
    font-weight: bolder;
    color: white;
}

.incorrect:hover {
    background: #d9534f;
    font-weight: bolder;
    color: white;
}

</style>