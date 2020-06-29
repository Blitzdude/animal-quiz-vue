<template>
	<div class="question-class-container">
		<b-jumbotron>
			<template slot="lead">
				{{ currentQuestion.question}}
			</template>

			
			<b-list-group>
				<b-list-group-item 
					v-for="(answer, index) in answers" 
					:key="index"
					@click="selectAnswer(index)"
				>
						{{answer}}
				</b-list-group-item>
			</b-list-group>

			<b-button variant="primary" href="#">Submit</b-button>
			<b-button @click="next()" variant="success" href="#">Next</b-button>
		</b-jumbotron>
	</div>
</template>

<script>
export default {
	props: {
		currentQuestion: Object,
		next: Function,
		correctAnswer: String,

	},
	computed: {
		answers() {
			let answers = [...this.currentQuestion.incorrect_answers];
			answers.push(this.currentQuestion.correct_answer);
			return answers;
		}
	},
	mounted() {
		console.log(this.currentQuestion);
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

.btn {
	margin: 0 5px;
}
</style>

