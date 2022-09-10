<script>
    let value = ""

    async function createShortUrl() {
		const res = await fetch(`https://shorty.johaneswiku.com/save`,{
            method: 'POST',
            cache: 'no-cache',
            headers: {
                'Content-Type' : 'application/json'
            },
            body: JSON.stringify({original_url: value})
        });

		const response = await res.json();

        //console.log(response)

		if (response.code == 200) {
			return `https://shorty.johaneswiku.com/${response.data.short_url}`;
		} else {
			throw new Error(response.status);
		}
	}
	
	let promise = null

	function handleClick() {
		promise = createShortUrl();
	}
</script>


<div class="input-wrapper">
    <input type="text" class="input-area" placeholder="https://example.com" bind:value>
    <div class="spacer"/>
    <button class="button-size" on:click={handleClick}>
        Short!
    </button>
</div>

{#if promise != null}
    {#await promise}
        <p>Creating URL ...</p>
    {:then value}
        <p>Your Short URL:  <a href={value} target="_blank" >{value}</a></p>
    {:catch error}
        <p style="color: red">{error.message}</p>
    {/await}
{/if}

<style>
    .input-wrapper{
        display: flex;
        align-items: center;
        justify-content: center;
    }
    .input-area{
        min-width: 30vw;
        min-height: 5vh;
        padding: 8px;
        font-size: 20px;
    }
    .spacer{
        width: 10px;
    }
    .button-size{
        min-height: 6vh;
        min-width: 5vw;
        padding: 8px;
    }
</style>