<script>
    import App from "../App.svelte";
    import Card from "../shared/Card.svelte"
    import {createEventDispatcher} from 'svelte'
    import PollStore from "../stores/PollStore";
    import Button from "../shared/Button.svelte";

    const dispatch = createEventDispatcher()
    export let poll;
    $: totalVotes = poll.votesA + poll.votesB
    $: percentA = Math.floor(poll.votesA/totalVotes*100)
    $: percentB = Math.floor(poll.votesB/totalVotes*100)

    const handleVote = (option, id) => {
    PollStore.update(currentPolls =>{
      let copiedPolls = [...currentPolls];
		let upvotedPoll = copiedPolls.find((poll) => poll.id == id);
      if (option === 'a') {
        upvotedPoll.votesA++;

      }
      if (option === 'b') {
        upvotedPoll.votesB++;

      }
		return copiedPolls;
    
    })  
  }

  const handleDelete = (id) => {
		// filter people out of array by id passed from handler function
    console.log(id)
    PollStore.update(currentPolls =>{
    
		return currentPolls.filter((poll) => poll.id != id);
    
    })
	}
</script>

<Card>
    <div class ="poll">
    <h3>{poll.question}</h3>
    <p>Total Votes: {totalVotes}</p>
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <div class = "answer" on:click={() => handleVote('a', poll.id)}>
        <div class = "percent percent-a" style="width: {percentA}%"></div>
            <span >{poll.answerA} ({Math.floor(poll.votesA/totalVotes*100)}%)</span>
        
    </div>
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <div class = "answer" on:click={() => handleVote('b', poll.id)}>
    <div class = "percent percent-b" style="width: {percentB}%"> </div>
        <span >{poll.answerB} ({Math.floor(poll.votesB/totalVotes*100)}%)</span>
   
    </div>
   <div class='delete'><Button on:click={() => handleDelete(poll.id)}>Delete</Button></div>
    </div>
    
</Card>
<style>
  h3{
    margin: 0 auto;
    color: #555;
  }
  p{
    margin-top: 6px;
    font-size: 14px;
    color: #aaa;
    margin-bottom: 30px;
  }
  .answer{
    background: #fafafa;
    cursor: pointer;
    margin: 10px auto;
    position: relative;
  }
  .answer:hover{
    opacity: 0.6;
  }
  span{
    display: inline-block;
    padding: 10px 20px;
  }
  .percent{
    height:100%;
    position:absolute;
    box-sizing: border-box;
  }
  .percent-a{
    border-left:4px solid red;
    background:rgba(217,27,66,0.2);
  }
  .percent-b{
    border-left:4px solid green;
    background: rgba(69,196,150,0.2);
}
</style>