<script>
    import { onMount } from "svelte";

    let albums = [];
    let selectedAlbum = null;

    // 获取专辑列表
    onMount(async () => {
        const response = await fetch("/apple-music-100-best-albums.json");
        albums = await response.json();
    });

    // 随机选择专辑
    function getRandomAlbum() {
        if (albums.length > 0) {
            const randomIndex = Math.floor(Math.random() * albums.length);
            selectedAlbum = albums[randomIndex];
        }
    }
</script>

<main>
    <h1>Shuffle</h1>
    <p>Listen without hesitation.</p>
    <button on:click={getRandomAlbum}>随机选一个专辑</button>

    {#if selectedAlbum}
        <div class="album">
            <h2>{selectedAlbum.name}</h2>
            <p><strong>Artist：</strong>{selectedAlbum.artist}</p>
            <p><strong>Year：</strong>{selectedAlbum.year}</p>
            <a href={selectedAlbum.link} target="_blank">More</a>
        </div>
    {/if}
</main>

<style>
    .album {
        margin-top: 20px;
        padding: 10px;
        border: 1px solid #ccc;
        border-radius: 5px;
        max-width: 300px;
    }
    button {
        margin-top: 10px;
        padding: 10px 20px;
        border: none;
        background-color: #007bff;
        color: white;
        border-radius: 5px;
        cursor: pointer;
    }
    button:hover {
        background-color: #0056b3;
    }
</style>
