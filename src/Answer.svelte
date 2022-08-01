<script>

    export let answerText;
    export let checkAnswerHandler;

</script>

<style>
    /* Beautify Answer*/
    .Answer {
        font-size: 2em;
        font-weight: bold;
        color: #0066ff;
        margin: 10px;
    }
</style>

<button on:click={() => checkAnswerHandler(answerText)} class="Answer">
    {answerText}
</button>

