<script>
	import { supabaseClient } from '$lib/db'
	import { invalidate, invalidateAll } from '$app/navigation'
	import { applyAction, enhance } from '$app/forms'
	import { page } from '$app/stores'
	import { onMount } from 'svelte'

	onMount(() => {
		const {
			data: { subscription }
		} = supabaseClient.auth.onAuthStateChange(() => {
			invalidate('supabase:auth')
		})

		return () => {
			subscription.unsubscribe()
		}
	})

	const logout = () => {
		return async ({ result }) => {
			await invalidateAll()
			applyAction(result)
		}
	}
</script>

<svelte:head>
	<title>Paeonia</title>
</svelte:head>

<div class='app'>
	<header>
		<div class='container'>
			<div style='display: flex;'>
				<div>
					<span style='color: #1273ff;'>Chronos</span>
					<span style='color: #ffd815;'>Project</span>
				</div>

				<div style='margin-left: auto;'>
					{#if $page.data.session}
						<form action='/logout' method='post' use:enhance={logout}>
							<button type='submit'>Sign Out</button>
						</form>
					{/if}
				</div>
			</div>
		</div>
	</header>
	
	<main>
		<div class='container'>
			<slot />
		</div>
	</main>
	
	<footer>
		<div class='container'>
			Footer
		</div>
	</footer>
</div>
