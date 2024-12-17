<script lang="ts">
	import { slide } from 'svelte/transition';
	import { cubicInOut } from 'svelte/easing';
	import {
		Home,
		Settings,
		Users,
		ChevronLeft,
		ChevronRight,
		LogOut,
		X,
		type IconProps
	} from 'lucide-svelte';
	import type { SvelteComponent } from 'svelte';

	// Props using $props
	const props = $props<{
		isMobile?: boolean;
		mobileOpen?: boolean;
		class_?: string;
		onStateChange?: (isOpen: boolean) => void;
	}>();

	// State declarations
	let expanded = $state(true);
	let isMobileOpen = $state(props.mobileOpen ?? false);
	const sidebarWidth = $derived(expanded ? 'w-64' : 'w-16');

	const user = $state({
		name: 'John Doe',
		email: 'john@example.com',
		initials: 'JD',
		image: null as string | null
	});

	// Navigation items
	const navItems: { icon: typeof SvelteComponent<IconProps>; label: string; href: string }[] = [
		{ icon: Home, label: 'Home', href: '/' },
		{ icon: Users, label: 'Users', href: '/users' },
		{ icon: Settings, label: 'Settings', href: '/settings' }
	];

	// Functions
	function toggleSidebar() {
		if (!props.isMobile) expanded = !expanded;
	}

	function closeMobileNav() {
		isMobileOpen = false;
		props.onStateChange?.(false);
	}

	// Add this reactive statement to sync with prop changes
	$effect(() => {
		isMobileOpen = props.mobileOpen ?? false;
	});
</script>

{#if props.isMobile && isMobileOpen}
	<!-- Overlay for mobile view -->
	<button
		type="button"
		aria-label="Close mobile navigation"
		class="fixed inset-0 z-40 bg-gray-600 bg-opacity-75 transition-opacity"
		onclick={closeMobileNav}
	>
	</button>
{/if}

<aside
	class={`${
		props.isMobile
			? 'fixed inset-y-0 left-0 z-50 w-64 transform transition-transform duration-300 ease-in-out'
			: `relative ${sidebarWidth} transition-all duration-300 ease-in-out`
	} h-screen border-r border-gray-200 bg-white dark:border-gray-700 dark:bg-gray-800 ${props.class_ ?? ''}`}
	class:translate-x-0={isMobileOpen}
	class:translate-x-[-100%]={props.isMobile && !isMobileOpen}
>
	<!-- Mobile Close Button -->
	{#if props.isMobile}
		<button
			class="absolute right-2 top-2 p-2 text-gray-500 hover:text-gray-700 dark:text-gray-400 dark:hover:text-gray-300"
			onclick={closeMobileNav}
		>
			<X size={20} />
		</button>
	{:else}
		<!-- Desktop Toggle Button -->
		<button
			class="absolute right-0 top-9 translate-x-1/2 flex h-6 w-6 items-center justify-center rounded-full border border-gray-200 bg-white text-gray-500 hover:bg-gray-50 dark:border-gray-700 dark:bg-gray-800 dark:text-gray-400 dark:hover:bg-gray-700"
			onclick={toggleSidebar}
			title={expanded ? 'Collapse sidebar' : 'Expand sidebar'}
		>
			{#if expanded}
				<ChevronLeft size={14} />
			{:else}
				<ChevronRight size={14} />
			{/if}
		</button>
	{/if}

	<div class="flex h-full flex-col">
		<!-- Logo -->
		<div class="p-4">
			{#if expanded}
				<div class="h-8 w-full rounded bg-gray-100 dark:bg-gray-700" />
			{:else}
				<div class="h-8 w-8 rounded bg-gray-100 dark:bg-gray-700" />
			{/if}
		</div>

		<!-- Navigation -->
		<nav class="flex-1 space-y-1 px-3 py-4">
			{#each navItems as item}
				<a
					href={item.href}
					class="group relative flex items-center rounded-lg px-2 py-2 text-gray-600 hover:bg-gray-100 dark:text-gray-300 dark:hover:bg-gray-700"
				>
					<svelte:component this={item.icon} size={20} />
					{#if expanded}
						<span class="ml-3" transition:slide={{ duration: 200, easing: cubicInOut }}>
							{item.label}
						</span>
					{:else}
						<div
							class="absolute left-full ml-2 hidden rounded bg-gray-900 px-2 py-1 text-xs text-white group-hover:block"
						>
							{item.label}
						</div>
					{/if}
				</a>
			{/each}
		</nav>

		<!-- User Profile -->
		<div class="border-t border-gray-200 p-4 dark:border-gray-700">
			<button class="flex w-full items-center">
				<div
					class="flex h-8 w-8 items-center justify-center rounded-full bg-gray-100 text-sm font-medium dark:bg-gray-700"
				>
					{#if user.image}
						<img src={user.image} alt={user.name} class="h-8 w-8 rounded-full" />
					{:else}
						{user.initials}
					{/if}
				</div>
				{#if expanded}
					<div class="ml-3 text-left">
						<p class="text-sm font-medium text-gray-700 dark:text-gray-200">{user.name}</p>
						<p class="text-xs text-gray-500 dark:text-gray-400">{user.email}</p>
					</div>
					<LogOut size={16} class="ml-auto text-gray-400" />
				{/if}
			</button>
		</div>
	</div>
</aside>
