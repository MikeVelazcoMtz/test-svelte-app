<svelte:head>
	<title>Movies - Svelte</title>
	<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
	<script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
	<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
	<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>
</svelte:head>
<script>
	import { onMount } from 'svelte';

	import Item from './Movie/Item.svelte';

	let name = 'world';
	let movies = [];
	let favoriteMovies = []
	const API_KEY = "29ed1d64cc3508c30f08131eb1860d99";
	const BASE_URL = 'https://api.themoviedb.org/3';
	const API_SETTINGS = `?api_key=${API_KEY}&language=es-MX`;

	function fetchMovies() {
		const URL = `${BASE_URL}/discover/movie${API_SETTINGS}&sort_by=popularity.desc`;
		return fetch(URL).then((res) => res.json()).then((data) => data);
	}

	function setLike (e) {
		favoriteMovies = [...favoriteMovies, e.detail];
	}

	function setDislike(e) {
		console.log(e.detail.id, favoriteMovies);
		favoriteMovies = favoriteMovies.filter((item) => item.id !== e.detail.id);
		console.log('AFTER', e.detail.id, favoriteMovies);
	}


	onMount(async () => {
		await fetchMovies().then((data) => movies = data.results);
	});
</script>

<h1>Hello {name}!</h1>
<main class="container">
	<div class="row">
		<div class="col-12 col-md-6 col-lg-8">
			<h1>Peliculas Populares</h1>
			<div class="row">
				{#each movies as movie}
					<div class="col-4">
						<Item {movie} on:message={setLike} />
					</div>
				{:else}
					Loading...
				{/each}
			</div>
		</div>
		<div class="col-12 col-md-6 col-lg-4">
			<h1>Peliculas Favoritas</h1>
			{#each favoriteMovies as movie (movie.id)}
				<div class="col-4">
					<Item {movie} on:message={setDislike} favorite={false} />
				</div>
				{:else}
					Sin peliculas favoritas
				{/each}
		</div>
	</div>
</main>

