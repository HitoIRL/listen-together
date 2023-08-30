<script lang="ts">
    import Fa from "svelte-fa";
    import { faTrashCan } from "@fortawesome/free-solid-svg-icons";

    export let thumbnail: string;
    export let title: string;
</script>

<li class="song">
    <img class="thumbnail" src={thumbnail} alt="Thumbnail">
    <p class="title">{title}</p>
    <div class="buttons">
        <Fa icon={faTrashCan}/>
    </div>
</li>

<style>
    .song {
        display: flex;
        text-align: left;
        align-items: center;
        gap: 15px;
    }

    .song:not(:last-child) {
        margin-bottom: 5px;
    }

    .thumbnail {
        width: 120px;
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
