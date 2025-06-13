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

const GITHUB_REPO_URL = 'https://github.com/Yuzu02/svelte5-showcase'
</script>

<div
    class="min-h-screen bg-gradient-to-br from-purple-900 via-blue-900 to-indigo-900"
>
    <!-- GitHub Icon - Fixed Position -->
    <div class="fixed top-2 right-2 sm:top-4 sm:right-4 z-50">
        <a
            href={GITHUB_REPO_URL}
            target="_blank"
            rel="noopener noreferrer"
            class="flex items-center justify-center w-10 h-10 sm:w-12 sm:h-12 bg-white/10 backdrop-blur-lg rounded-full border border-white/20 hover:bg-white/20 transition-all duration-300 group"
            aria-label="View on GitHub"
        >
            <svg
                class="w-5 h-5 sm:w-6 sm:h-6 text-white group-hover:scale-110 transition-transform duration-300"
                fill="currentColor"
                viewBox="0 0 24 24"
            >
                <path
                    d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"
                />
            </svg>
        </a>
    </div>
    <div class="container mx-auto px-4 sm:px-6 lg:px-8 py-4 sm:py-8">
        <!-- Header -->
        <header class="text-center mb-6 sm:mb-8">
            <h1
                class="text-3xl sm:text-4xl md:text-5xl lg:text-6xl font-bold bg-gradient-to-r from-pink-400 to-purple-400 bg-clip-text text-transparent mb-3 sm:mb-4 px-2"
            >
                🚀 Svelte 5 Showcase
            </h1>
            <p
                class="text-base sm:text-lg md:text-xl text-gray-300 max-w-2xl mx-auto px-4"
            >
                Explore all the amazing new features in Svelte 5: Runes,
                Snippets, Enhanced State Management, and more!
            </p>
        </header>
        <!-- Navigation Tabs -->
        <nav class="flex justify-center mb-6 sm:mb-8 px-2">
            <div
                class="bg-white/10 backdrop-blur-lg rounded-xl sm:rounded-full p-1 sm:p-2 border border-white/20 w-full sm:w-auto overflow-x-auto sm:overflow-visible"
            >
                <div
                    class="flex space-x-1 sm:space-x-2 min-w-max sm:min-w-0 justify-start sm:justify-center"
                >
                    {#each _tabs as tab}
                        <button
                            class="px-3 py-2 sm:px-4 md:px-6 sm:py-3 rounded-lg sm:rounded-full transition-all duration-300 flex items-center space-x-1 sm:space-x-2 text-xs sm:text-sm md:text-base whitespace-nowrap
								{_activeTab === tab.id
                                ? 'bg-white text-gray-900 shadow-lg'
                                : 'text-gray-300 hover:text-white hover:bg-white/10'}"
                            onclick={() => (_activeTab = tab.id)}
                        >
                            <span class="text-sm sm:text-lg">{tab.icon}</span>
                            <span class="font-medium hidden sm:inline"
                                >{tab.label}</span
                            >
                            <span class="font-medium sm:hidden"
                                >{tab.label.split(" ")[0]}</span
                            >
                        </button>
                    {/each}
                </div>
            </div>
        </nav>
        <!-- Content Area -->
        <main
            class="bg-white/5 backdrop-blur-lg rounded-2xl sm:rounded-3xl border border-white/10 p-4 sm:p-6 md:p-8 shadow-2xl mx-2 sm:mx-0"
        >
            {#if _activeTab === "runes"}
                <RunesDemo />
            {:else if _activeTab === "snippets"}
                <SnippetsDemo />
            {:else if _activeTab === "state"}
                <StateDemo />
            {:else if _activeTab === "effects"}
                <EffectsDemo />
            {:else if _activeTab === "events"}
                <EventHandlerDemo />
            {/if}
        </main>
        <!-- Footer -->
        <footer class="text-center mt-8 sm:mt-12 text-gray-400 px-4">
            <div class="flex flex-col items-center space-y-3">
                <p class="text-sm sm:text-base">
                    Built with ❤️ using Svelte 5, SvelteKit, Bun, and Tailwind
                    CSS
                </p>
                <div
                    class="flex flex-col sm:flex-row items-center space-y-2 sm:space-y-0 sm:space-x-4"
                >
                    <a
                        href={GITHUB_REPO_URL}
                        target="_blank"
                        rel="noopener noreferrer"
                        class="flex items-center space-x-2 text-gray-400 hover:text-white transition-colors duration-200 text-sm sm:text-base"
                    >
                        <svg
                            class="w-4 h-4"
                            fill="currentColor"
                            viewBox="0 0 24 24"
                        >
                            <path
                                d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"
                            />
                        </svg>
                        <span>View Source</span>
                    </a>
                    <span class="text-gray-600 hidden sm:inline">•</span>
                    <a
                        href="https://svelte.dev"
                        target="_blank"
                        rel="noopener noreferrer"
                        class="text-gray-400 hover:text-orange-400 transition-colors duration-200 text-sm sm:text-base"
                    >
                        Svelte 5
                    </a>
                </div>
            </div>
        </footer>
    </div>
</div>
