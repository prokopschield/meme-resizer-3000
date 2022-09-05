<script lang="ts">
    let files: FileList;

    let input_url: string = "";

    let loading = false;

    let last_error = "";

    let width = "420";
    let height = "420";

    async function change() {
        loading = true;

        for (const file of files) {
            try {
                const response = await fetch("/put", {
                    method: "PUT",
                    headers: {
                        "Content-Type": file.type,
                        "Content-Length": String(file.size),
                    },
                    body: file,
                });

                const { url } = await response.json();

                input_url = url;
            } catch (error) {
                last_error = error;
            }
        }

        loading = false;
    }

    function resize() {
        location.href = `https://webshot.nodesite.eu/rest/${width}/${height}/full/${input_url}`;
    }
</script>

<main>
    <h1>Epic Meme Resizer 3000</h1>
    <label for="input_url">Input URL</label>
    <input id="input_url" bind:value={input_url} />
    <br />
    <input type="file" bind:files on:change={change} />
    <br />
    <label for="width">Width:</label>
    <input id="width" bind:value={width} />
    <br />
    <label for="width">Height:</label>
    <input id="height" bind:value={height} />
    <br />
    {#if !loading}
        <button on:click={resize}>Resize!</button>
    {:else}
        <b>{last_error || "Loading!"}</b>
    {/if}
</main>

<style>
</style>
