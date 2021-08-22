<script>
    import PollStore from '../Stores/PollStore.js';
    import Button from '../Shared/Button.svelte';
    import { createEventDispatcher } from 'svelte';

    const dispatch = createEventDispatcher();

    var fields = {
        'question': '',
        'answer-a': '',
        'answer-b': ''
    }

    var errors = { 'question': '', 'answer-a': '', 'answer-b': '' }
    var valid = false;

    function submitHandler() {
        valid = true;
        
        if (fields['question'].trim().length < 5) {
            valid = false;
            errors['question'] = 'Question must be at least 5 characters long.'
        }
        else {
            errors['question'] = '';
        }

        if (fields['answer-a'].trim().length < 1) {
            valid = false;
            errors['answer-a'] = 'Answer A cannot be empty.'
        }
        else {
            errors['answer-a'] = '';
        }

        if (fields['answer-b'].trim().length < 1) {
            valid = false;
            errors['answer-b'] = 'Answer B cannot be empty.'
        }
        else {
            errors['answer-b'] = '';
        }
        
        if (valid) {
            const poll = {...fields, votesA: 0, votesB: 0, id: Math.random()}
            PollStore.update(currentPolls => {
                return [poll, ...currentPolls];
            })
            dispatch('add')
        }
    }
</script>

<form on:submit|preventDefault={submitHandler}>
    <div class="form-field">
        <label for="question">Poll Question:</label>
        <input type="text" id="question" bind:value={fields['question']}>  
    </div>

    <div class="error">{ errors['question'] }</div>

    <div class="form-field">
        <label for="question">Answer A:</label>
        <input type="text" id="answer-a" bind:value={fields['answer-a']}>
    </div>

    <div class="error">{ errors['answer-a'] }</div>

    <div class="form-field">
        <label for="question">Answer B:</label>
        <input type="text" id="answer-b" bind:value={fields['answer-b']}>
    </div>

    <div class="error">{ errors['answer-b'] }</div><br>

    <Button type='secondary' flat={true}>Add Poll</Button>
</form>

<style>
    form {
        width: 400px;
        margin: 0 auto;
        text-align: center;
    }

    .form-field {
        margin: 18px auto;
    }

    input {
        width: 100%;
        border-raidus: 6px;
    }

    label {
        margin: 10px auto;
        text-align: left;
    }

    .error {
        font-weight: bold;
        font-size: 12px;
        color: #d91b42;
    }
</style>