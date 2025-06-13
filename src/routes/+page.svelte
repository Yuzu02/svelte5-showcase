<script>
import '../app.css'
import EffectsDemo from '../lib/components/EffectsDemo.svelte'
import EventHandlerDemo from '../lib/components/EventHandlerDemo.svelte'
import RunesDemo from '../lib/components/RunesDemo.svelte'
import SnippetsDemo from '../lib/components/SnippetsDemo.svelte'
import StateDemo from '../lib/components/StateDemo.svelte'

let _activeTab = $state('runes')

const _tabs = [
	{ id: 'runes', label: 'Runes System', icon: '🪄' },
	{ id: 'snippets', label: 'Snippets', icon: '📄' },
	{ id: 'state', label: 'State Management', icon: '🔄' },
	{ id: 'effects', label: 'Effects', icon: '⚡' },
	{ id: 'events', label: 'Event Handlers', icon: '🎯' }
]
</script>

<div class="min-h-screen bg-gradient-to-br from-purple-900 via-blue-900 to-indigo-900">
	<div class="container mx-auto px-4 py-8">
		<!-- Header -->
		<header class="text-center mb-12">
			<h1
				class="text-6xl font-bold bg-gradient-to-r from-pink-400 to-purple-400 bg-clip-text text-transparent mb-4"
			>
				🚀 Svelte 5 Showcase
			</h1>
			<p class="text-xl text-gray-300 max-w-2xl mx-auto">
				Explore all the amazing new features in Svelte 5: Runes, Snippets, Enhanced State
				Management, and more!
			</p>
		</header>

		<!-- Navigation Tabs -->
		<nav class="flex justify-center mb-8">
			<div class="bg-white/10 backdrop-blur-lg rounded-full p-2 border border-white/20">
				<div class="flex space-x-2">
					{#each _tabs as tab}
						<button
							class="px-6 py-3 rounded-full transition-all duration-300 flex items-center space-x-2
								{_activeTab === tab.id
								? 'bg-white text-gray-900 shadow-lg'
								: 'text-gray-300 hover:text-white hover:bg-white/10'}"
							onclick={() => (_activeTab = tab.id)}
						>
							<span class="text-lg">{tab.icon}</span>
							<span class="font-medium">{tab.label}</span>
						</button>
					{/each}
				</div>
			</div>
		</nav>

		<!-- Content Area -->
		<main class="bg-white/5 backdrop-blur-lg rounded-3xl border border-white/10 p-8 shadow-2xl">
			{#if _activeTab === 'runes'}
				<RunesDemo />
			{:else if _activeTab === 'snippets'}
				<SnippetsDemo />
			{:else if _activeTab === 'state'}
				<StateDemo />
			{:else if _activeTab === 'effects'}
				<EffectsDemo />
			{:else if _activeTab === 'events'}
				<EventHandlerDemo />
			{/if}
		</main>

		<!-- Footer -->
		<footer class="text-center mt-12 text-gray-400">
			<p>Built with ❤️ using Svelte 5, SvelteKit, Bun, and Tailwind CSS</p>
		</footer>
	</div>
</div>
