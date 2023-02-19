<script>
	import { applyAction, enhance } from '$app/forms'

	export let form
	let loading = false

	const handleSubmit = () => {
		loading = true
		return async ({ result }) => {
			loading = false
			await applyAction(result)
		}
	}
</script>

{#if form?.error}
	{form.error}
{/if}

{#if form?.message}
	{form.message}
{/if}

<form method='post' use:enhance={handleSubmit}>
	<div>
		<label for='email'>Email</label>
		<p class='control'>
			<input
				id='email'
				name='email'
				value={form?.values?.email ?? ''}
				type='email'
				placeholder='Email'
				required
			/>
		</p>
	</div>
	
	<div>
		<p>
			<button disabled={loading}>Login</button>
		</p>
	</div>
</form>
