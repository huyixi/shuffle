<script>
    import { onMount } from "svelte";

    let albums = [];
    let selectedAlbum = null;

    // 获取专辑列表
    onMount(async () => {
        const response = await fetch("/apple-music-100-best-albums.json");
        albums = await response.json();
        getRandomAlbum();
    });

    // 随机选择专辑
    function getRandomAlbum() {
        if (albums.length > 0) {
            const randomIndex = Math.floor(Math.random() * albums.length);
            selectedAlbum = albums[randomIndex];
            selectedAlbum.appleMusicLink =
                getAppleMusicSearchLink(selectedAlbum);
        }
    }

    function getAppleMusicSearchLink(album) {
        if (!album) return "";
        const query = `${album.name}`;
        return `https://music.apple.com/us/search?term=${encodeURIComponent(query)}`;
    }

    function getAppleMusicDeeplink(album) {
        if (!album) return "";
        const query = `${album.name} ${album.artist}`;
        return `music://search?term=${encodeURIComponent(query)}`;
    }
</script>

<main>
    <h1>Shuffle</h1>
    <p>Listen without hesitation.</p>
    <button on:click={getRandomAlbum}>Shuffle</button>

    {#if selectedAlbum}
        <div class="album">
            <h2>{selectedAlbum.album}</h2>
            <p><strong>Artist：</strong>{selectedAlbum.artist}</p>
            <p><strong>Year：</strong>{selectedAlbum.year}</p>
            <p>
                <a href={getAppleMusicDeeplink(selectedAlbum)} target="_blank"
                    >open in Apple Music</a
                >
            </p>

            <p>
                <a href={selectedAlbum.link} target="_blank">Wiki Detail</a>
            </p>
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
