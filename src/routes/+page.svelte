<script lang="ts">
	import { goto } from "$app/navigation";

    async function createSession() {
        const response = await fetch("http://127.0.0.1:3000/session", {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
        });
        const json = await response.json();

        goto(`/session/${json.id}`);
    }
</script>

<h1 class="title">Listen Together</h1>
<p class="desc">Listen to music with your friends simultaneously<br>from different services for free!</p>
<input on:click={createSession} class="button" type="button" value="Create Session">
<p class="supported">(Currently supported streaming services are YouTube, Spotify and SoundCloud)</p>
<p class="watermark">Created fully from scratch by <a class="watermark-link" href="https://github.com/HitoIRL" target="_blank">@HitoIRL</a></p>

<style>
    .title {
        font-size: 3rem;
        margin-bottom: 0;
    }

    .desc {
        font-size: 1.5rem;
        margin-top: 0;
        text-align: center;
    }

    .button {
        font-size: 1.5rem;
        padding: 1rem 2rem;
        border-radius: 1rem;
        border: none;

        cursor: pointer;
        color: white;
        background-color: var(--accent-color);
        /* transition: background-color 0.2s ease-in-out; */
    } 

    .button:hover {
        background-color: var(--light-accent-color);
    }

    .supported {
        color: #6e6e6e;
        text-align: center;
    }

    .watermark {
        position: absolute;
        bottom: 0;
    }

    .watermark-link {
        color: var(--accent-color);
    }
</style>
