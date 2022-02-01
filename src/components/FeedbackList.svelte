<script>
    import { scale, fade } from "svelte/transition";
    import {FeedbackStore} from '../stores';
    import FeedbackItem from "./FeedbackItem.svelte";


    // export let feedback = [];
    // FeedbackStore.subscribe(data => feedback = data) // get data from store and save it in local var
    // let feedback = [];


</script>

{#each $FeedbackStore as item (item.id)}
    <div in:scale out:fade="{{ duration: 500 }}">
        <FeedbackItem {item} />
    </div>
{/each}
