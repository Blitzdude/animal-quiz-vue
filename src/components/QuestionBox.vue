<template>
	<div class="question-class-container">
		<b-jumbotron>
			<template slot="lead">
				{{ currentQuestion.question}}
			</template>

			<b-list-group>
				<b-list-group-item 
					v-for="(answer, index) in shuffledAnswers" 
					:key="index"
					:disabled="answered"
					@click="selectAnswer(index)"
					:class="answerClass(index)"
				>
						{{answer}}
				</b-list-group-item>
			</b-list-group>

			<b-button 
				variant="primary" 
				@click="submitAnswer"
				:disabled="selectedIndex === null || answered"
			>
			Submit
			</b-button>
			<b-button :disabled="disableNext" @click="next()" variant="success" href="#">Next</b-button>
		</b-jumbotron>
	</div>
</template>

<script>
import _ from 'lodash'
export default {
	data() {
		return {
			selectedIndex: null,
			correctIndex: null,
			shuffledAnswers: [],
      answered: false,
		}
	},
	props: {
		currentQuestion: Object,
    next: Function,
    disableNext: Boolean,
		correctAnswer: String,
		increment: Function
	},
	computed: {
		answers() {
			let answers = [...this.currentQuestion.incorrect_answers];
			answers.push(this.currentQuestion.correct_answer);
			return answers;
		}
	},
	watch: {
		currentQuestion: {
			immediate: true,
			handler() {
				this.selectedIndex = null;
				this.answered = false;
				this.shuffleAnswers();
			}
		}
	},
	methods: {
		selectAnswer(index) {
			console.log("Selected: ", index);
			this.selectedIndex = index;
		},
		submitAnswer() {
			let isCorrect = false;

			if (this.selectedIndex === this.correctIndex) {
				isCorrect = true;	
			}
			this.answered = true;

			this.increment(isCorrect)
		},
		shuffleAnswers() {
			let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer];
			this.shuffledAnswers = _.shuffle(answers)
			this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
		},
		answerClass(index) {
			let answerClass = '';

			if (!this.answered && this.selectedIndex === index) {
				answerClass = 'selected'
			}
			else if (this.answered && this.correctIndex === index) {
				answerClass = 'correct'
			}
			else if (this.answered && this.correctIndex !== index && this.selectedIndex === index) {
				answerClass = 'incorrect'
			}
			return answerClass;
		}
	}
}
</script>


<style scoped>

.question-class-container {
	text-align: center;
}

.list-group {
	margin-bottom: 20px;
}

.list-group-item:hover {
	background-color: rgb(189, 189, 189);
}

.list-group-item:active {
	background-color: rgba(189, 189, 189);
}

.btn {
	margin: 0 5px;
}

.selected {
	background-color: lightblue;
}

.correct {
	background-color: lightgreen;
}

.incorrect {
	background-color: lightcoral;
}
</style>

