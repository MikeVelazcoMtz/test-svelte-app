<script>
	import { createEventDispatcher } from "svelte";
	const dispatch = createEventDispatcher();
	export let movie;
	export let favorite = true;

	let { poster_path, original_title, title, overview, id } = movie;
	$: coverURL = `https://image.tmdb.org/t/p/w185_and_h278_bestv2${poster_path}`;

	function toggleLike() {
		dispatch("message", {
			id,
			title,
			original_title,
			overview,
			poster_path,
		});
	}
</script>

<div class="card" style="width: 12rem;">
	<img src={coverURL} class="card-img-top" alt={original_title} />
	<div class="card-body">
		<h5 class="card-title">{title} ({original_title})</h5>
		<p class="card-text">{overview}</p>
		<div class="text-center">
			<button class="btn btn-primary" on:click={toggleLike}>
				{favorite ? "like" : "dislike"}
			</button>
		</div>
	</div>
</div>

<style>
	.card-img-top {
		width: 100%;
		height: 250px;
	}
</style>
