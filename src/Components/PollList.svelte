<script>
    import { fade, slide, scale } from 'svelte/transition';
    import { flip } from 'svelte/animate';
    import PollStore from '../Stores/PollStore.js';
    import PollDetails from './PollDetails.svelte';
    import Card from '../Shared/Card.svelte';
</script>

<div class="poll-list">
    {#each $PollStore as poll (poll.id)}
    <div in:fade out:scale|local animate:flip={{duration: 500}}>
        <Card>
            <PollDetails {poll} on:vote/>
        </Card>
    </div>
    {/each}
</div>

<style>
    .poll-list {
        display: grid;
        grid-template-columns: 1fr 1fr;
        grid-gap: 20px;
    }
</style>