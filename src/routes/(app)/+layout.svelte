<script lang="ts">
	import TopNav from '$lib/components/TopNav.svelte';
	import Sidebar from '$lib/components/Sidebar.svelte';

	let { children } = $props();
	let mobileNavOpen = $state(false);

	function toggleMobileNav() {
		mobileNavOpen = !mobileNavOpen;
	}

	function handleSidebarStateChange(isOpen: boolean) {
		mobileNavOpen = isOpen;
	}
</script>

<div class="flex min-h-screen flex-col md:flex-row">
	<Sidebar isMobile={false} mobileOpen={false} class_="hidden md:block" />
	<Sidebar
		isMobile={true}
		mobileOpen={mobileNavOpen}
		onStateChange={handleSidebarStateChange}
		class_="md:hidden"
	/>

	<div class="flex-1">
		<TopNav handleToggle={toggleMobileNav} />
		<main class="container p-4">
			{@render children()}
		</main>
	</div>
</div>
