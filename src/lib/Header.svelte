<script lang="ts">
	import Menu from './Menu.svelte';
</script>

<header class="the-header">
	<a href="/" class="the-header__logo">
		<span aria-hidden="true" class="the-header__logo-emblem">3C</span>
		<span>3 Pillar <strong>Canban</strong></span>
	</a>
	<Menu />
</header>

<style>
	.the-header {
		position: sticky;
		top: 0;
		padding: 2rem;
		backdrop-filter: blur(1px);
		background: rgba(255, 255, 255, 0.1);
		display: flex;
		justify-content: space-between;
		border-bottom: 1px solid #1f3a8a0f;
	}

	.the-header__logo {
		color: var(--primary-color);
		font-weight: 800;
		text-decoration: none;
		font-size: 1.5rem;
		text-transform: uppercase;
		display: flex;
		white-space: nowrap;
		gap: 0.8rem;
	}

	.the-header__logo-emblem {
		background: var(--primary-color);
		color: #fff;
		aspect-ratio: 1/1;
		border-radius: 100%;
		display: grid;
		place-items: center;
		font-size: 1rem;
	}

	.the-header__logo strong {
		color: var(--text-color);
		display: block;
		font-size: 0.7em;
	}
</style>
