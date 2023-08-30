<script lang="ts">
	import Fa from "svelte-fa";
	import { faBackward, faForward, faPause, faPlay } from "@fortawesome/free-solid-svg-icons";
    
    import Song from "./Song.svelte";

    let isPaused = false;

    let url = "";
    function addSong(event: KeyboardEvent) {
        if (event.key !== "Enter") {
            return;
        }

        console.log(`Adding song: ${url}`);
    }

    let songs: { title: string, thumbnail: string, url: string }[] = [
        { title: "Koza x Oorbit - Delfin", thumbnail: "https://i.ytimg.com/vi/5qap5aO4i9A/maxresdefault.jpg", url: "https://www.youtube.com/watch?v=xxd8-cx90s0" },
        { title: "Koza x Oorbit - Delfin", thumbnail: "https://i.ytimg.com/vi/5qap5aO4i9A/maxresdefault.jpg", url: "https://www.youtube.com/watch?v=xxd8-cx90s0" },
        { title: "Koza x Oorbit - Delfin", thumbnail: "https://i.ytimg.com/vi/5qap5aO4i9A/maxresdefault.jpg", url: "https://www.youtube.com/watch?v=xxd8-cx90s0" },
        { title: "Koza x Oorbit - Delfin", thumbnail: "https://i.ytimg.com/vi/5qap5aO4i9A/maxresdefault.jpg", url: "https://www.youtube.com/watch?v=xxd8-cx90s0" },
        { title: "Koza x Oorbit - Delfin", thumbnail: "https://i.ytimg.com/vi/5qap5aO4i9A/maxresdefault.jpg", url: "https://www.youtube.com/watch?v=xxd8-cx90s0" },
    ];

    /*
	import { goto } from "$app/navigation";
	import { page } from "$app/stores";
	import { onMount } from "svelte";

    import Song from "./Song.svelte";

    interface Packet {
        kind: "AddSong" | "SetSongs",
        data: string,
    }

    let songs: any[] = [];
    function processPacket(packet: Packet) {
        console.log(packet);

        switch (packet.kind) {
            case "SetSongs":
                let data = JSON.parse(packet.data);
                songs = data;
                break;
        }
    }

    let ws: WebSocket;
    onMount(() => {
        const id = $page.params.id;

        ws = new WebSocket(`ws://127.0.0.1:3000/session/${id}`);

        ws.onerror = (event) => {
            // we can assume that the session doesn't exist
            goto("/");
        }

        ws.onopen = () => {
            console.log("Connected to WebSocket");
        }

        ws.onmessage = (event) => {
            const packet = JSON.parse(event.data) as Packet;
            processPacket(packet);
        }

        ws.onclose = () => {
            console.log("Disconnected from WebSocket");
        }
    });

    let url = "";
    async function addSong() {
        const packet: Packet = {
            kind: "AddSong",
            data: url,
        };
        ws.send(JSON.stringify(packet));
        url = "";
    }
    */
</script>

<div class="wrapper">
    <div class="controls">
        {#if songs.length === 0}
            <h2>There are no songs in the queue</h2>
        {:else}
            <h2 class="current-song">{songs[0].title}</h2>
            <div class="buttons">
                <div class="button">
                    <Fa fw icon={faBackward} size="2x"/>
                </div>
                <div on:click={() => isPaused = !isPaused} class="button main-button">
                    {#if isPaused}
                        <Fa fw icon={faPlay} size="2x"/>
                    {:else}
                        <Fa fw icon={faPause} size="2x"/>
                    {/if}
                </div>
                <div class="button">
                    <Fa fw icon={faForward} size="2x"/>
                </div>
            </div>
            <progress class="progress" value="32" max="100">32%</progress>
        {/if}
    </div>
    <div class="queue">
        <input bind:value={url} on:keypress={addSong} class="queue-input" type="text" name="url" placeholder="Song URL">
        {#each songs as song}
            <Song title={song.title} thumbnail={song.thumbnail}/>
        {/each}
    </div>
</div>

<style>
    .wrapper {
        display: flex;
        gap: 15px;
        height: 400px;

        padding: 50px;

        color: white;
        background-color: #121212;
        text-align: center;
    }

    .controls {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        gap: 30px;
        background-color: #000;
        padding: 30px;
        width: 600px;
    }

    .current-song {
        max-width: 400px;
        overflow: hidden;
        text-overflow: ellipsis;
        margin: 0 0 20px 0;
    }

    .buttons {
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 20px;
    }

    .button {
        cursor: pointer;
    }

    .main-button {
        background-color: var(--accent-color);
        padding: 10px;
        border-radius: 5px;
    }

    .main-button:hover {
        background-color: var(--light-accent-color);
    }

    .progress {
        width: 100%;
    }

    .progress[value] {
        -webkit-appearance: none;
        appearance: none;
        -moz-appearance: none;

        height: 20px;
        border: none;

        background-color: rgb(35, 35, 35);
    }

    .progress[value]::-moz-progress-bar,
    .progress[value]::-webkit-progress-bar {
        background-color: white;
        box-shadow: 0 0 3px black inset;
    }

    .queue {
        width: 400px;
        overflow-y: scroll;
        background-color: #000;
        padding: 10px;
    }

    .queue-input {
        box-sizing: border-box;
        width: 100%;
        background-color: transparent;
        border: 2px solid rgb(49, 49, 49);
        outline: none;
        color: white;
        padding: 15px;
        font-size: 20px;
        margin-bottom: 15px;
    }

    .queue-input:focus {
        border: 2px solid var(--accent-color);
    }
</style>
