<script lang="ts">
	import { Menu } from 'lucide-svelte';

	const props = $props<{
		handleToggle: () => void;
	}>();
</script>

<nav class="border-b border-gray-200 bg-white dark:border-gray-700 dark:bg-gray-800">
	<div class="px-4 py-3">
		<div class="flex items-center justify-between">
			<div class="flex items-center">
				<span class="text-xl font-semibold text-gray-800 dark:text-white">App Name</span>
			</div>

			<button
				type="button"
				class="md:hidden rounded-lg p-2 text-gray-500 hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-gray-200 dark:text-gray-400 dark:hover:bg-gray-700 dark:focus:ring-gray-600"
				onclick={props.handleToggle}
			>
				<Menu size={24} />
			</button>
		</div>
	</div>
</nav>
