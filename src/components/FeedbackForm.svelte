<script>
    import Button from "./Button.svelte";
    import Card from "./Card.svelte";
    import RatingSelect from "./RatingSelect.svelte";
    import {FeedbackStore} from '../stores';

    import {v4 as uuidv4} from 'uuid'; // for custom IDs!!

    let text = "";
    let btnDisabled = true;
    let minimDigit = 10;
    let message = "";
    let rating = 10;


    const handleInput = () => {
        if (text.trim().length <= minimDigit) {
            message = `Text must be at least ${minimDigit} characters`;
            btnDisabled = true;
        } else {
            message = null;
            btnDisabled = false;
        }
    };

    const handleSelect = (e) => rating = e.detail

    const handleSubmit = (e) => {
        if(text.trim().length > minimDigit) {
            const newFeedback = {
                id: uuidv4(),
                text,
                rating: +rating // to be sure that rating is a number add +
            }
            FeedbackStore.update((currentFeedback) => [newFeedback, ...currentFeedback])
            text = ""
        }
    }

</script>

<Card>
    <header>
        <h2>How would you rate your service with us?</h2>
    </header>
    <form on:submit|preventDefault={handleSubmit}>
        <RatingSelect on:rating-select={handleSelect}/>
        <div class="input-group">
            <input
                type="text"
                placeholder="Tell us something that keeps you coming back!"
                bind:value={text}
                on:input={handleInput}
            />
            <Button type="submit" disabled={btnDisabled}>Send</Button>
        </div>
    </form>
    {#if message}
        <div class="message">{message}</div>
    {/if}
</Card>

<style>
    header {
        max-width: 400px;
        margin: auto;
    }
    header h2 {
        font-size: 22px;
        font-weight: 600;
        text-align: center;
    }
    .input-group {
        display: flex;
        flex-direction: row;
        border: 1px solid #ccc;
        padding: 8px 10px;
        border-radius: 8px;
        margin-top: 15px;
    }
    input {
        flex-grow: 2;
        border: none;
        font-size: 16px;
    }
    input:focus {
        outline: none;
    }
    .message {
        padding-top: 10px;
        text-align: center;
        color: rebeccapurple;
    }
</style>
