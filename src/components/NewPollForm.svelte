<script>
    import Button from '../shared/Button.svelte'
    import {createEventDispatcher} from 'svelte'
    import PollStore from '../stores/PollStore'
    const dispatch = createEventDispatcher()

    let fields = {question:'', answerA:'', answerB:''};
    let errors = {question:'', answerA:'', answerB:''};
    let valid = false;

    const submitHandler = () => {
        valid = true;
        if (fields.question.trim().length < 5) {
            valid = false;
            errors.question = "question must be at least 5 characters"
        }
        else{
            errors.question = ''
        }
        if(fields.answerA.trim().length < 5){ 
            valid = false;
            errors.answerA = "Answer must be at least 5 characters"
        }
        else{
            errors.answerA = ''
        }
        if(fields.answerB.trim().length < 5){ 
            valid = false;
            errors.answerB = "Answer must be at least 5 characters"
        }
        else{
            errors.answerB = ''
        }
        if (valid){
        console.log(fields)
        let poll = {...fields, votesA:0, votesB:0, id:Math.random()}
        PollStore.update(currentPolls=>{
            return [poll, ...currentPolls]
        })
        dispatch('newPoll', poll)
        }
    }
</script>
<form on:submit|preventDefault={submitHandler}>
    <div class="form-field">
        <label for="question">Poll Question:</label>
        <input type="text" id="question" bind:value={fields.question}>
        <div class="error">{errors.question}</div>
    </div>
    <div class="form-field">
        <label for="answer-a">Answer A:</label>
        <input type="text" id="answer-a" bind:value={fields.answerA}>
        <div class="error">{errors.answerA}</div>
    </div>
    <div class="form-field">
        <label for="answer-b">Answer B:</label>
        <input type="text" id="answer-b" bind:value={fields.answerB}>
        <div class="error">{errors.answerB}</div>
    </div>
<Button type ='secondary' flat={false}>Submit</Button>
</form>
<style>
    form{
        width: 400px;
        margin: 0 auto;
        text-align: center;
    }
    .form-field{
        margin: 10px auto;

    }
    input{
        width: 100%;
        border-radius: 6px;
    }
    label{
        margin: 10px auto;
        text-align: left;
    }
    .error{
        font-weight:bold ;
        color: red;
        font-size: 12px;
    }
</style>