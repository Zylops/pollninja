<script>
	import Header from './Components/Header.svelte';
	import Footer from './Components/Footer.svelte';
	import Tabs from './Shared/Tabs.svelte';
	import CreatePollForm from './Components/CreatePollForm.svelte';
	import PollList from './Components/PollList.svelte';

	import { tweened } from 'svelte/motion';
	const value = tweened(0)

	var items = ['Current Polls', 'Add New Poll']
	var activeItem = items[0];

	function tabChange(e) {
		activeItem = e.detail;
	}

	function handleAdd() {
		activeItem = items[0];
	}

</script>

<Header/>

<main>

	<Tabs {activeItem} {items} on:tabChange={tabChange}/>

	{#if activeItem == items[0]}
		<PollList/>
	{:else if activeItem == items[1]}
		<CreatePollForm on:add={handleAdd}/>
	{/if}

</main>

<Footer/>

<style>

main {
	max-width: 900px;
	margin: 40px auto 40px auto;
}

</style>