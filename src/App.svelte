<script>
	import Question from "./Question.svelte";
	import Answer from "./Answer.svelte";
	import Points from "./Points.svelte";
	let start = false;
	let quiz = [];
	let amount;
	let difficulty;
	let category;
	async function onSubmit(e) {
		const formData = new FormData(e.target);
		const data = {};
		for (let field of formData) {
			const [key, value] = field;
			data[key] = value;
		}
		amount = data.count;
		difficulty = data.difficulty;
		category = data.category;
		await getQuestions();
		start = true;
		console.log(quiz);
  	}
	let color = "blue";
	function unescapeHtml(safe) {
    return safe.replace(/&amp;/g, '&')
        .replace(/&lt;/g, '<')
        .replace(/&gt;/g, '>')
        .replace(/&quot;/g, '"')
        .replace(/&#039;/g, "'");
	}
	
	async function getQuestions() {
		const response = await fetch("https://opentdb.com/api.php?amount="+amount+"&category="+category+"&difficulty="+difficulty+"&type=boolean")
		const data = await response.json();
		let results = data.results;
		for (let i = 0; i < results.length; i++) {
			let q = results[i];
			let question = unescapeHtml(q.question);
			let correctAnswer = "True" === q.correct_answer ? "Yes" : "No";
			let answers = ['Yes', 'No'];
			// Add to Quiz
			quiz.push({
				question: question,
				answers: answers,
				correctAnswer: correctAnswer
			});
		}
	}
	let points = 0;
	let quizIndex = 0;
	function checkAnswerHandler(answerText) {
		const isCorrect = answerText === quiz[quizIndex].correctAnswer;
		if (isCorrect) {
			points++;
			quiz[quizIndex].question = 'Correct!'
			color = "green";
		}
		else{
			quiz[quizIndex].question = 'Wrong :('
			color = "red";
		}
		setTimeout(function() {
			if (quizIndex+1 >= quiz.length) {
				console.log("Quiz Over");
				setTimeout(function() {
					color = "black";
					quiz[quizIndex].question = 'Game Over!, You got ' + points + ' points!';
					quizIndex = 0;
					points = 0;
				}, 2000)
			}
			else{
				color = "blue";
				quizIndex++;
			}
		}, 2000)
	}

</script>

<style>
	.App {
		margin-left:auto;
		margin-right:auto;
		height:auto; 
		width:auto;
	}
	.answers {
		display: flex;
		flex-direction: row;
        position: absolute;
        top: 70%;
        left: 50%;
        transform: translate(-50%, -50%);
        text-align: center;
	}
	/* Place in centre of page */
	.AppBeforeStart {
		position: fixed;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
	}
</style>

{#if start === true}
<div class="App">
	<Points points={points}/>
	<Question questionText={quiz[quizIndex].question} color={color}/>
	<div class="answers">
		<Answer answerText={quiz[quizIndex].answers[0]} checkAnswerHandler={checkAnswerHandler}/>
		<Answer answerText={quiz[quizIndex].answers[1]} checkAnswerHandler={checkAnswerHandler}/>
	</div>
</div>
{:else}
<div class="AppBeforeStart">
	<h1>Quizz...</h1>
	<form  on:submit|preventDefault={onSubmit}>
		<!-- Drop Down category -->
		<select name="category">
			<option value="9">General Knowledge</option>
			<option value="10">Entertainment: Books</option>
			<option value="11">Entertainment: Film</option>
			<option value="12">Entertainment: Music</option>
			<option value="13">Entertainment: Musicals</option>
			<option value="14">Entertainment: Television</option>
			<option value="15">Entertainment: Video Games</option>
			<option value="16">Entertainment: Board Games</option>
			<option value="17">Science & Nature</option>
			<option value="18">Science: Computers</option>
			<option value="19">Science: Mathematics</option>
			<option value="20">Mythology</option>
			<option value="21">Sports</option>
			<option value="22">Geography</option>
			<option value="23">History</option>
			<option value="24">Politics</option>
			<option value="25">Art</option>
			<option value="26">Celebrities</option>
			<option value="27">Animals</option>
			<option value="28">Vehicles</option>
			<option value="29">Entertainment: Comics</option>
			<option value="30">Science: Gadgets</option>
			<option value="31">Entertainment: Japanese Anime & Manga</option>
			<option value="32">Entertainment: Cartoon & Animations</option>
		</select>
		<!-- Drop Down difficulty -->
		<select name="difficulty">
			<option value="easy">Easy</option>
			<option value="medium">Medium</option>
			<option value="hard">Hard</option>
		</select>
		<!-- Number of Questions (Options 5, 10, 20) -->
		<select name="count">
			<option value="5">5</option>
			<option value="10">10</option>
			<option value="20">20</option>
		</select>
		<!-- Submit Button -->
		<button type="submit">Start</button>
	</form>
</div>
{/if}