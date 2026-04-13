<script lang="ts">
	import { onMount } from 'svelte';
	import { goto } from '$app/navigation';
	import { page } from '$app/stores';
	
	import NotebookList from '$lib/components/notebooks/NotebookList.svelte';
	import NotebookChat from '$lib/components/notebooks/NotebookChat.svelte';
	import NotebookSearch from '$lib/components/notebooks/NotebookSearch.svelte';
	
	let activeTab = 'notebooks';
	let selectedNotebookId = null;
	
	$: if ($page.url.pathname.startsWith('/research/chat/')) {
		activeTab = 'chat';
		selectedNotebookId = $page.url.pathname.split('/').pop();
	} else if ($page.url.pathname.startsWith('/research/search')) {
		activeTab = 'search';
	} else {
		activeTab = 'notebooks';
	}
	
	const handleTabChange = (tab: string) => {
		activeTab = tab;
		if (tab === 'notebooks') {
			goto('/research');
		} else if (tab === 'search') {
			goto('/research/search');
		}
	};
	
	const handleNotebookSelect = (notebookId: string) => {
		selectedNotebookId = notebookId;
		goto(`/research/chat/${notebookId}`);
	};
</script>

<div class="flex h-screen bg-gray-50 dark:bg-gray-900">
	<!-- Sidebar Navigation -->
	<div class="w-64 bg-white dark:bg-gray-800 border-r border-gray-200 dark:border-gray-700">
		<div class="p-4">
			<h2 class="text-lg font-semibold text-gray-800 dark:text-gray-200 mb-4">Research</h2>
			<nav class="space-y-2">
				<button
					class="w-full text-left px-3 py-2 rounded-lg transition-colors {activeTab === 'notebooks' ? 'bg-blue-100 dark:bg-blue-900 text-blue-700 dark:text-blue-300' : 'hover:bg-gray-100 dark:hover:bg-gray-700 text-gray-700 dark:text-gray-300'}"
					on:click={() => handleTabChange('notebooks')}
				>
					Notebooks
				</button>
				<button
					class="w-full text-left px-3 py-2 rounded-lg transition-colors {activeTab === 'search' ? 'bg-blue-100 dark:bg-blue-900 text-blue-700 dark:text-blue-300' : 'hover:bg-gray-100 dark:hover:bg-gray-700 text-gray-700 dark:text-gray-300'}"
					on:click={() => handleTabChange('search')}
				>
					Search
				</button>
				{#if selectedNotebookId}
					<button
						class="w-full text-left px-3 py-2 rounded-lg transition-colors {activeTab === 'chat' ? 'bg-blue-100 dark:bg-blue-900 text-blue-700 dark:text-blue-300' : 'hover:bg-gray-100 dark:hover:bg-gray-700 text-gray-700 dark:text-gray-300'}"
						on:click={() => handleTabChange('chat')}
					>
						Chat
					</button>
				{/if}
			</nav>
		</div>
	</div>
	
	<!-- Main Content -->
	<div class="flex-1 overflow-hidden">
		{#if activeTab === 'notebooks'}
			<NotebookList on:select={handleNotebookSelect} />
		{:else if activeTab === 'search'}
			<NotebookSearch />
		{:else if activeTab === 'chat'}
			<NotebookChat notebookId={selectedNotebookId} />
		{/if}
	</div>
</div>

<style>
	/* Add any additional styles here */
</style>
