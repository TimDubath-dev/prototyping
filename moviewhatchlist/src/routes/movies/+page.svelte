<script>
	import { page } from '$app/stores';
	import { movies } from '$lib/data/movies';

	let searchQuery = $derived($page.url.searchParams.get('search') || '');

	let filteredMovies = $derived(
		searchQuery
			? movies.filter(movie =>
				movie.title.toLowerCase().includes(searchQuery.toLowerCase())
			)
			: movies
	);
</script>

<div class="row mb-4">
	<div class="col-12">
		<h1>Movie Collection</h1>
		{#if searchQuery}
			<p class="text-muted">Search results for: "{searchQuery}"</p>
		{/if}
	</div>
</div>

<div class="row g-4">
	{#each filteredMovies as movie}
		<div class="col-12 col-sm-6 col-md-4 col-lg-3">
			<div class="card h-100 shadow-sm">
				<img src={movie.image} class="card-img-top" alt={movie.title} style="height: 400px; object-fit: cover;" />
				<div class="card-body d-flex flex-column">
					<h5 class="card-title">{movie.title}</h5>
					<div class="mt-auto">
						<p class="card-text mb-1">
							<strong>Year:</strong> {movie.year}
						</p>
						<p class="card-text mb-0">
							<strong>Duration:</strong> {movie.duration} min
						</p>
					</div>
				</div>
			</div>
		</div>
	{:else}
		<div class="col-12">
			<div class="alert alert-info text-center" role="alert">
				No movies found.
			</div>
		</div>
	{/each}
</div>
