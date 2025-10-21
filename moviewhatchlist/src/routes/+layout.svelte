<script>
	import favicon from '$lib/assets/favicon.svg';
	import { movies } from '$lib/data/movies';
	import { goto } from '$app/navigation';

	let { children } = $props();
	let searchQuery = $state('');
	let showResults = $state(false);

	let searchResults = $derived(
		searchQuery.trim()
			? movies.filter(movie =>
				movie.title.toLowerCase().includes(searchQuery.toLowerCase())
			)
			: []
	);

	function handleSearch(event) {
		event.preventDefault();
		if (searchQuery.trim()) {
			showResults = false;
			goto(`/movies?search=${encodeURIComponent(searchQuery)}`);
		}
	}

	function selectMovie(movie) {
		searchQuery = movie.title;
		showResults = false;
		goto(`/movies?search=${encodeURIComponent(movie.title)}`);
	}

	function handleInput() {
		showResults = searchQuery.trim().length > 0;
	}

	function handleBlur() {
		// Delay to allow click on dropdown item
		setTimeout(() => {
			showResults = false;
		}, 200);
	}
</script>

<svelte:head>
	<link rel="icon" href={favicon} />
	<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet" />
	<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
</svelte:head>

<nav class="navbar navbar-expand-lg navbar-dark bg-dark">
	<div class="container">
		<a class="navbar-brand" href="/">ScreenStack</a>
		<button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
			<span class="navbar-toggler-icon"></span>
		</button>
		<div class="collapse navbar-collapse" id="navbarNav">
			<ul class="navbar-nav me-auto">
				<li class="nav-item">
					<a class="nav-link" href="/movies">Movies</a>
				</li>
			</ul>
			<form class="d-flex position-relative" onsubmit={handleSearch}>
				<input
					class="form-control me-2"
					type="search"
					placeholder="Search movies..."
					bind:value={searchQuery}
					oninput={handleInput}
					onblur={handleBlur}
					onfocus={handleInput}
				/>
				<button class="btn btn-outline-success" type="submit">Search</button>

				{#if showResults && searchResults.length > 0}
					<div class="search-results position-absolute bg-white border rounded shadow-lg" style="top: 100%; left: 0; right: 70px; max-height: 400px; overflow-y: auto; z-index: 1050;">
						{#each searchResults as movie}
							<button
								type="button"
								class="dropdown-item d-flex align-items-center p-2 border-bottom"
								onclick={() => selectMovie(movie)}
							>
								<img src={movie.image} alt={movie.title} class="me-2" style="width: 50px; height: 75px; object-fit: cover;" />
								<div class="text-start">
									<div class="fw-bold text-dark">{movie.title}</div>
									<small class="text-muted">{movie.year} • {movie.duration} min</small>
								</div>
							</button>
						{/each}
					</div>
				{:else if showResults && searchQuery.trim()}
					<div class="search-results position-absolute bg-white border rounded shadow-lg" style="top: 100%; left: 0; right: 70px; z-index: 1050;">
						<div class="p-3 text-muted text-center">No movies found</div>
					</div>
				{/if}
			</form>
		</div>
	</div>
</nav>

<main class="container my-4">
	{@render children?.()}
</main>

<style>
	.search-results {
		margin-top: 0.25rem;
	}

	.dropdown-item:hover {
		background-color: #f8f9fa;
		cursor: pointer;
	}
</style>
