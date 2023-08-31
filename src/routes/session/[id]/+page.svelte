<script lang="ts">
	import { onMount } from "svelte";
	import { page } from "$app/stores";
	import { goto } from "$app/navigation";
	import Fa from "svelte-fa";
	import { faBackward, faForward, faPause, faPlay, faTrash, faTrashCan, faVolumeHigh } from "@fortawesome/free-solid-svg-icons";
    
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
    function addSong(event: KeyboardEvent) {
        if (event.key !== "Enter") {
            return;
        }

        console.log(`Adding song: ${url}`);
        const packet: Packet = {
            kind: "AddSong",
            data: url,
        };
        ws.send(JSON.stringify(packet));
        url = "";
    }

    let isPaused = true;
    let duration = 1.0;
    let currentTime = 0.0;
    let volume = 0.2;
</script>

<div class="wrapper">
    <div class="listeners">
        <h2 class="listeners-heading">Listeners</h2>
        <ul class="listeners-list">
            <li>HitoIRL</li>
        </ul>
    </div>
    <div class="controls">
        {#if songs.length === 0}
            <h2>There are no songs in the queue</h2>
        {:else}
            <audio class="audio-controller" bind:volume={volume} bind:duration={duration} bind:currentTime={currentTime} bind:paused={isPaused} controls>
                <source src={songs[0].audio} type="audio/webm">
                    Your browser does not support the audio element.
            </audio>
            <h2 class="current-song">{songs[0].title}</h2>
            <div class="buttons">
                <div class="button">
                    <Fa fw icon={faBackward} size="2x"/>
                </div>
                {#if isPaused}
                    <button class="button main-button" on:click={() => isPaused = false}>
                        <Fa fw icon={faPlay} size="2x"/>
                    </button>
                {:else}
                    <button class="button main-button" on:click={() => isPaused = true}>
                        <Fa fw icon={faPause} size="2x"/>
                    </button>
                {/if}
                <div class="button">
                    <Fa fw icon={faForward} size="2x"/>
                </div>
            </div>
            <input bind:value={currentTime} class="input-range progress" type="range" name="progress" max={duration}>
            <div class="volume-row">
                <Fa icon={faVolumeHigh} size="1.5x"/>
                <input bind:value={volume} class="input-range" type="range" name="volume" max="1.0" step="0.01">
            </div>
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

        background-color: #121212;
    }

    .listeners {
        background-color: #000;
        padding: 30px;
        width: 200px;
        overflow-y: scroll;
    }

    .listeners-heading {
        margin: 0;
        text-align: center;
    }

    .listeners-list {
        list-style-type: none;
        padding: 0;
    }

    .listeners-list li {
        background-color: rgb(27, 27, 27);
        padding: 10px;
    }

    .listeners-list li:not(:last-child) {
        margin-bottom: 5px;
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

    .audio-controller {
        display: none;
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
        border: none;
        color: white;
    }

    .main-button {
        background-color: var(--accent-color);
        padding: 10px;
        border-radius: 5px;
    }

    .main-button:hover {
        background-color: var(--light-accent-color);
    }

    .input-range {
        -webkit-appearance: none;
        appearance: none;
        background: transparent;
        cursor: pointer;
    }

    .input-range::-webkit-slider-runnable-track,
    .input-range::-moz-range-track {
        background: rgb(35, 35, 35);
        height: 20px;
    }

    .input-range::-webkit-slider-thumb,
    .input-range::-moz-range-thumb {
        -webkit-appearance: none;
        appearance: none;

        border: none;
        background-color: transparent;
    }

    .input-range::-moz-range-progress {
        background-color: white;
        box-shadow: 0 0 3px black inset;
        height: 20px;
    }

    .progress {
        width: 100%;
    }

    .volume-row {
        display: flex;
        align-items: center;
        gap: 15px;
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
