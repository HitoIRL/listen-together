<script lang="ts">
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
    }
</script>

<div class="wrapper">
    <div class="box listeners"></div>
    <div class="box controls"></div>
    <div class="box queue">
        <div class="input-row">
            <input bind:value={url} class="input song-input" type="text" name="url" placeholder="Song URL">
            <input on:click={addSong} class="input add-input" type="button" value="+">
        </div>
        <ul class="songs">
            {#each songs as song}
                <Song title={song.title} thumbnail={song.thumbnail}/>
            {/each}
        </ul>
    </div>
</div>

<style>
    .wrapper {
        display: flex;
        width: 100%;
        height: 100%;

        padding: 50px;
        box-sizing: border-box;
        gap: 15px;
    }

    .box {
        box-sizing: border-box;
    }

    .listeners {
        width: 20%;
    }

    .controls {
        width: 50%;
    }

    .queue {
        width: 30%;
    }

    .input-row {
        display: flex;
    }

    .input {
        box-sizing: border-box;
        outline: none;
        border: none;
    }

    .song-input {
        width: 100%;
        padding: 10px;
        font-size: 1.5rem;
        
        border-left: 2px solid black;
        border-bottom: 2px solid black;
        border-top: 2px solid black;
    }

    .song-input:focus {
        border: 2px solid var(--accent-color);
    }

    .add-input {
        cursor: pointer;
        width: 60px;

        font-size: 2rem;
        font-weight: bold;
        background-color: var(--accent-color);

        border-bottom: 2px solid black;
        border-top: 2px solid black;
        border-right: 2px solid black;
    }

    .add-input:hover {
        background-color: var(--light-accent-color);
    }

    .songs {
        list-style-type: none;
        padding: 0;
    }
</style>
