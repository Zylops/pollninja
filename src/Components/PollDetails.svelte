<script>
    import PollStore from '../Stores/PollStore.js';
    import Button from '../Shared/Button.svelte';

    export var poll;

    // Reactive Values
    $: totalVotes = poll['votesA'] + poll['votesB']
    $: percentA = Math.floor(100 / totalVotes * poll.votesA)
    $: percentB = Math.floor(100 / totalVotes * poll.votesB)

    function handleVote (option , id) {

        PollStore.update(currentPolls => {
            var pollsDupe = [...currentPolls];
            var upvotedPoll = pollsDupe.find((poll) => poll.id == id);

            if (option === 'a') {
                upvotedPoll.votesA++;
            }

            if (option === 'b') {
                upvotedPoll.votesB++;
            }

            return pollsDupe;
        });

    }

    function handleDelete(id) {
        
        PollStore.update((currentPolls) => {
            return currentPolls.filter(poll => poll.id != id)
        })

    }

</script>

<div class="poll">
    <h3>{ poll['question'] }</h3>
    <p>{totalVotes} votes.</p>

    <div class="answer" on:click={() => handleVote('a', poll.id)}>
        <div class="percent" class:percent-b={poll.votesA >= poll.votesB} class:percent-a={poll.votesA <= poll.votesB} style="width: {percentA}%"></div>
        <span>{ poll['answer-a'] } ({ poll['votesA'] })</span>
    </div>

    <div class="answer" on:click={() => handleVote('b', poll.id)}>
        <div class="percent percent-b" class:percent-b={poll.votesB >= poll.votesA} class:percent-a={poll.votesB <= poll.votesA} style="width: {percentB}%"></div>
        <span>{ poll['answer-b'] } ({ poll['votesB'] })</span>
    </div>

    <div class="delete">
        <Button flat={true} on:click={() => {handleDelete(poll.id)}}>Delete</Button>
    </div>
</div>

<style>
    h3 {
        margin: 0 auto;
        color: #555;
    }

    p {
        margin-top: 6px;
        font-size: 14px;
        color: #aaa;
        margin-bottom: 30px;
    }

    .answer {
        background: #fafafa;
        cursor: pointer;
        margin: 10px auto;
        position: relative;
    }

    .answer:hover {
        opacity: 0.6;
    }

    span {
        display: inline-block;
        padding: 10px 20px;
    }

    .percent {
        transition: width 500ms;
        height: 100%;
        position: absolute;
        box-sizing: border-box;
    }

    .percent-a {
        border-left: 4px solid #d91b42;
        background-color: rgba(217, 27, 66, 0.2);
    }

    .percent-b {
        border-left: 4px solid #45c496;
        background-color: rgba(69, 196, 150, 0.2);
    }

    .delete {
        margin-top: 30px;
        text-align: center;
    }
</style>