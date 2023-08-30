<script lang="ts">
    import "normalize.css";
</script>

<div class="main">
    <slot/>
</div>

<style>
    @import url("https://fonts.googleapis.com/css2?family=Bricolage+Grotesque:opsz,wght@10..48,400;10..48,700&display=swap");

    :global(:root) {
        --accent-color: #1db954;
        --light-accent-color: #1ed760;
    }

    .main {
        font-family: "Bricolage Grotesque", sans-serif;
        background-color: #000;

        display: flex;
        align-items: center;
        justify-content: center;

        height: 100vh;
    }
</style>
