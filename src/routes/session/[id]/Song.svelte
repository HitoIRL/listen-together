<script lang="ts">
    import Fa from "svelte-fa";
    import { faTrashCan } from "@fortawesome/free-solid-svg-icons";

    export let thumbnail: string;
    export let title: string;
</script>

<li class="song">
    <img class="thumbnail" src={thumbnail} alt="Thumbnail">
    <h3>{title}</h3>
</li>

<style>
    .song {
        display: flex;
        align-items: center;
        padding: 10px;
        gap: 15px;
        border: 2px solid black;
    }

    .song:not(:last-child) {
        margin-bottom: 5px;
    }

    .thumbnail {
        width: 120px;
    }
</style>
