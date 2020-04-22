<template>
    <div class="question-box-container">
        <b-jumbotron>
            <template v-slot:header>
                Science Test
                <hr class="my-4 hr-half" />
            </template>

            <template v-slot:lead>
                {{decodeHtml(currentQuestion.question)}}
            </template>

            <hr class="my-4" />
            
            <b-list-group label="Answers">
                <b-list-group-item 
                    v-for="(answer, index) in shuffledAnswers" 
                    :key="index" 
                    @click.prevent="selectAnswer(index)"
                    :class="answerClass(index)"
                >
                    {{ decodeHtml(answer) }}
                </b-list-group-item>
            </b-list-group>

            <hr class="my-4" />

            <b-button 
                variant="primary"
                @click="submitAnswer"
                :disabled="selectedIndex === null || answered"
            >
            <!-- Check if the question has answered or if there is even anything selected -->
                Submit
            </b-button>
            <b-button 
                variant="success" 
                @click="next" 
                :disabled="index === 19"
            >
                Next
            </b-button>
        </b-jumbotron>
    </div>
</template>

<script>
import _ from "lodash";
export default {
    props: {
        currentQuestion: Object,
        next: Function,
        increment: Function,
        index: Number
    },
    data(){
        return {
            selectedIndex: null,
            correctIndex: null,
            shuffledAnswers: [],
            answered: false
        }
    },
    //NO LONGER USED
    // computed: {
    //     answers(){
    //         let answers = [...this.currentQuestion.incorrect_answers]
    //         answers.push(this.currentQuestion.correct_answer)
    //         return answers
    //     }
    // },
    watch: {
        currentQuestion: {
            // Set immediate true so that it shuffles/displays the first answers
            immediate: true,
            handler(){
                this.selectedIndex = null;
                this.answered = false;
                this.shuffleAnswers();
            }
        } 
    },
    methods: {
        selectAnswer(index){
            this.selectedIndex = index;
        },
        shuffleAnswers() {
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
            this.shuffledAnswers = _.shuffle(answers);
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
        },
        submitAnswer(){
            let isCorrect = false;
            if (this.selectedIndex === this.correctIndex){
                isCorrect = true;
            }
            this.answered = true;

            this.increment(isCorrect);
        },
        answerClass(index){
            let answerClass = '';
            if (!this.answered && this.selectedIndex === index){
                answerClass = 'selected';
            } else if (this.answered && this.correctIndex === index ){
                answerClass = 'correct';
            } else if (this.answered && this.selectedIndex === index &&
                this.correctIndex !== index){
                answerClass = 'incorrect';
            }
            return answerClass;
        },
        decodeHtml: function (html) {
            var txt = document.createElement("textarea");
            txt.innerHTML = html;
            return txt.value;
        }
    }
}
</script>

<style scoped>
    .hr-half{
        width: 33%;
    }
    .display-3{
        font-size: 40px;
    }
    .list-group{
        margin-bottom: 15px;
    }
    .list-group-item:hover{
        background-color: #EEE;
        cursor: pointer;
        color: #343a40;
    }
    .list-group-item.selected:hover{
        background-color: rgb(105, 122, 168);
        cursor: pointer;
        color: white;
    }
    .list-group-item.correct:hover{
        background-color: rgb(113, 172, 113);
        cursor: pointer;
        color: white;
    }
    .list-group-item.incorrect:hover{
        background-color: rgb(209, 85, 85);
        cursor: pointer;
        color: white;
    }
    .btn{
        margin: 0px 5px 0px 5px;
    }
    .selected{
        background-color: rgb(46, 72, 187);
        color: white;
    }
    .correct{
        background-color: rgb(13, 155, 13);
        color: white;
    }
    .incorrect{
        background-color: rgb(226, 0, 0);
        color: white;
    }
</style>