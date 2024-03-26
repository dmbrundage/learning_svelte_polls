<script>
	import Header from './components/Header.svelte';
	import Footer from './components/Footer.svelte';
	import Tabs from './shared/Tabs.svelte';
	import NewPollForm from './components/NewPollForm.svelte';
	import PollList from './components/PollList.svelte'
	//tabs
	let items = ["Current Polls", "Add New Poll"];
	let activeItem = "Current Polls";

	const updateActiveItem = (e) =>{
		
		activeItem = e.detail
	}

	const updateNewPoll = (e) =>{
		activeItem = "Current Polls"
	}

	const handleVote = (e) =>{
		
	}
</script>
<Header/>
<main>
	<Tabs {items} {activeItem} on:updateActiveItem={updateActiveItem}/>
	{#if activeItem === "Current Polls"}
	<PollList  on:vote={handleVote}/>
	
	{:else}
	<NewPollForm on:newPoll={updateNewPoll}/>
	
	{/if}
</main>
<Footer />
<style>
	main{
		max-width: 960px;
		margin: 40px auto;
	}
</style>