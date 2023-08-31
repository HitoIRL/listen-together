<script lang="ts">
    import Fa from "svelte-fa";
    import { faTrashCan } from "@fortawesome/free-solid-svg-icons";

    export let thumbnail: string;
    export let title: string;
    export let current: boolean;
    
    console.log(current);
</script>

<li class="song" style:--color={current ? "var(--accent-color)" : "none"}>
    <img class="thumbnail" src={thumbnail} alt="Thumbnail">
    <p class="title">{title}</p>
    <div class="buttons" on:click>
        <Fa icon={faTrashCan}/>
    </div>
</li>

<style>
    .song {
        display: flex;
        text-align: left;
        align-items: center;
        gap: 15px;
        height: 90px;
        background: var(--color);
    }

    .song:not(:last-child) {
        margin-bottom: 5px;
    }

    .thumbnail {
        width: 120px;
        height: 100%;
        object-fit: cover;
    }

    .title {
        flex-grow: 1;

        overflow: hidden;
        text-overflow: ellipsis;
    }

    .buttons {
        margin: 0 20px 0 10px;
        cursor: pointer;
    }
</style>
