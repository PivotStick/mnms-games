<script>
	import '@fortawesome/fontawesome-free/css/all.min.css';
	import '../app.scss';
	import { page } from '$app/stores';

	let links = [
		{
			href: '/tictactoe',
			icon: 'fa-table-cells',
			name: 'Tic Tac Toe'
		},
		{
			href: '/pacman',
			icon: 'fa-ghost',
			name: 'PacMan'
		}
	];
</script>

<div class="container">
	<nav>
		{#each links as link (link.href)}
			<a href={link.href} aria-current={link.href === $page.url.pathname ? 'page' : undefined}>
				<i class="fa {link.icon}"></i>
				{link.name}
			</a>
		{/each}
	</nav>

	<main>
		<slot />
	</main>
</div>

<style lang="scss">
	.container {
		height: 100vh;
		overflow: auto;
		display: grid;
		grid-template-rows: auto 1fr;
	}

	nav {
		background: var(--color-950);
		color: var(--on-color-950);

		padding: 1rem 2rem;

		display: flex;
		align-items: center;
		gap: 1rem;

		a {
			opacity: 0.75;
			transition-property: opacity, background-color;

			padding: 0.5rem 1rem;
			border-radius: 2rem;

			display: flex;
			align-items: baseline;
			gap: 0.5rem;

			&[aria-current='page'] {
				background-color: var(--color-50);
				color: var(--on-color-50);
				pointer-events: none;
			}

			&:hover {
				opacity: 1;
			}

			&:active {
				opacity: 0.5;
			}
		}
	}

	main {
		height: 100%;
		display: flex;
		flex-direction: column;
		gap: 3rem;
		justify-content: center;
		align-items: center;
	}
</style>
