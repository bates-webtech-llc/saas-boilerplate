<script lang="ts">
	import type { PageData } from './$types.js';
	import Login from '$lib/components/login/Login.svelte';

	let { data }: { data: PageData } = $props();
</script>

<div class="flex min-h-screen flex-col lg:flex-row">
	<div
		class="flex w-full items-center justify-center px-4 py-12 sm:px-6 lg:w-1/2 lg:px-20 xl:px-24"
	>
		<div class="w-full max-w-sm">
			{#if data.user}
				<a href="/" class="btn btn-primary font-semibold text-md md:text-lg mt-4 w-full max-w-sm"
					>Continue with current account
				</a>
			{:else}
				<Login data={data.form} />
			{/if}
		</div>
	</div>
	<div class="relative hidden w-full lg:block lg:w-1/2">
		<img
			class="absolute inset-0 h-full w-full object-cover"
			src="https://images.unsplash.com/photo-1496917756835-20cb06e75b4e?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1974&q=80"
			alt="Login background"
		/>
	</div>
</div>
