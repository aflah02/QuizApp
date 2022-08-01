<script>

    export let questionText;
    export let color;

</script>

<style>
    /* Center QuesDiv*/
    .QuesDiv {
        position: absolute;
        top: 25%;
        left: 50%;
        transform: translate(-50%, -50%);
        text-align: center;
    }
    /* Beautify Question*/
	.Question {
        font-size: 3em;
        font-weight: bold;
	}
</style>

<div class="QuesDiv">
    <h1 class="Question" style="color:{color}">
        {questionText}
    </h1>
</div>