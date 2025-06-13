<script lang="ts">
import { tick, untrack } from 'svelte'

let count = $state(0)
let name = $state('')
let isVisible = $state(true)
let messages: string[] = $state([])
let effectLogs: Array<{ id: number; message: string; type: string; timestamp: string }> = $state([])

// Timer state
let _seconds = $state(0)
let isTimerRunning = $state(false)
let timerInterval: number | null = null

// DOM reference
let logContainer: HTMLElement | null = null
function addLog(message: string, type = 'info') {
	untrack(() => {
		effectLogs.push({
			id: Date.now(),
			message,
			type,
			timestamp: new Date().toLocaleTimeString()
		})

		// Keep only last 10 logs
		if (effectLogs.length > 10) {
			effectLogs.shift()
		}
	})
}

// Basic effect - runs when dependencies change
$effect(() => {
	addLog(`Count changed to: ${count}`, 'count')
})

// Effect with cleanup
$effect(() => {
	if (name) {
		addLog(`Hello ${name}!`, 'greeting')

		// Cleanup function
		return () => {
			addLog(`Goodbye ${name}!`, 'cleanup')
		}
	}
})

// Effect.pre - runs before DOM updates
$effect.pre(() => {
	if (count > 0) {
		addLog(`Pre-update: Count will be ${count}`, 'pre')
	}
})

// Complex effect with multiple dependencies
$effect(() => {
	const message = `Status: Count=${count}, Name=${name || 'Anonymous'}, Visible=${isVisible}`
	addLog(message, 'status')
})

// Effect for timer
$effect(() => {
	if (isTimerRunning) {
		timerInterval = setInterval(() => {
			_seconds++
		}, 1000)

		addLog('Timer started', 'timer')

		// Cleanup
		return () => {
			if (timerInterval) {
				clearInterval(timerInterval)
				timerInterval = null
				addLog('Timer stopped', 'timer')
			}
		}
	}
})

// Effect with untrack to avoid dependencies
$effect(() => {
	if (count > 0) {
		// This won't create a dependency on 'name'
		untrack(() => {
			if (name) {
				addLog(`Untracked: Count is ${count}, but name dependency is ignored`, 'untrack')
			}
		})
	}
})

// Effect that runs only once (like onMount)
$effect(() => {
	addLog('Component mounted - this runs only once', 'mount')

	return () => {
		addLog('Component will unmount', 'unmount')
	}
})

// Effect for DOM manipulation
$effect(() => {
	if (logContainer && effectLogs.length > 0) {
		// Scroll to bottom when new logs are added
		tick().then(() => {
			if (logContainer) {
				logContainer.scrollTop = logContainer.scrollHeight
			}
		})
	}
})

// Functions
function increment() {
	count++
}

function decrement() {
	count--
}

function reset() {
	count = 0
	name = ''
	_seconds = 0
	isTimerRunning = false
}

function toggleTimer() {
	isTimerRunning = !isTimerRunning
}

function toggleVisibility() {
	isVisible = !isVisible
}

function clearLogs() {
	effectLogs.length = 0
}

function addMessage() {
	messages.push(`Message ${messages.length + 1}`)
}
</script>

<div class="space-y-8">
	<div class="text-center">
		<h2 class="text-4xl font-bold text-white mb-4">⚡ Effects System</h2>
		<p class="text-gray-300 text-lg">
			Explore $effect, $effect.pre, cleanup functions, and side effect management in Svelte 5.
		</p>
	</div>

	<!-- Control Panel -->
	<div class="bg-white/10 rounded-2xl p-6 border border-white/20">
		<h3 class="text-2xl font-semibold text-blue-300 mb-4">🎛️ Control Panel</h3>

		<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
			<div class="bg-black/30 rounded-lg p-4">
				<p class="block text-white mb-2">Counter: {count}</p>
				<div class="flex space-x-2">
					<button
						class="bg-red-500 hover:bg-red-600 text-white px-3 py-2 rounded-lg transition-colors"
						onclick={decrement}
					>
						-
					</button>
					<button
						class="bg-green-500 hover:bg-green-600 text-white px-3 py-2 rounded-lg transition-colors"
						onclick={increment}
					>
						+
					</button>
				</div>
			</div>

			<div class="bg-black/30 rounded-lg p-4">
				<label for="name-input" class="block text-white mb-2">Name:</label>
				<input
					id="name-input"
					class="w-full px-3 py-2 bg-black/50 border border-gray-600 rounded-lg text-white focus:border-blue-500 focus:outline-none"
					type="text"
					bind:value={name}
					placeholder="Your name"
				/>
			</div>
			<div class="bg-black/30 rounded-lg p-4">
				<p class="block text-white mb-2">Timer: {_seconds}s</p>
				<button
					class="w-full {isTimerRunning
						? 'bg-red-500 hover:bg-red-600'
						: 'bg-green-500 hover:bg-green-600'} text-white px-4 py-2 rounded-lg transition-colors"
					onclick={toggleTimer}
				>
					{isTimerRunning ? 'Stop' : 'Start'} Timer
				</button>
			</div>

			<div class="bg-black/30 rounded-lg p-4">
				<p class="block text-white mb-2">Visibility:</p>
				<button
					class="w-full {isVisible
						? 'bg-blue-500 hover:bg-blue-600'
						: 'bg-gray-500 hover:bg-gray-600'} text-white px-4 py-2 rounded-lg transition-colors"
					onclick={toggleVisibility}
				>
					{isVisible ? 'Hide' : 'Show'}
				</button>
			</div>
		</div>

		<div class="flex justify-center space-x-4 mt-4">
			<button
				class="bg-purple-500 hover:bg-purple-600 text-white px-6 py-2 rounded-lg transition-colors"
				onclick={reset}
			>
				Reset All
			</button>
			<button
				class="bg-orange-500 hover:bg-orange-600 text-white px-6 py-2 rounded-lg transition-colors"
				onclick={clearLogs}
			>
				Clear Logs
			</button>
		</div>
	</div>

	<!-- Effect Logs -->
	<div class="bg-white/10 rounded-2xl p-6 border border-white/20">
		<h3 class="text-2xl font-semibold text-green-300 mb-4">📋 Effect Logs</h3>
		<p class="text-gray-400 mb-4">Watch how effects trigger when state changes:</p>

		<div
			bind:this={logContainer}
			class="bg-black/40 rounded-lg p-4 h-64 overflow-y-auto border border-gray-600"
		>
			{#each effectLogs as log}
				<div
					class="mb-2 p-2 rounded text-sm
					{log.type === 'count' ? 'bg-blue-500/20 text-blue-300' : ''}
					{log.type === 'greeting' ? 'bg-green-500/20 text-green-300' : ''}
					{log.type === 'cleanup' ? 'bg-red-500/20 text-red-300' : ''}
					{log.type === 'pre' ? 'bg-yellow-500/20 text-yellow-300' : ''}
					{log.type === 'status' ? 'bg-purple-500/20 text-purple-300' : ''}
					{log.type === 'timer' ? 'bg-orange-500/20 text-orange-300' : ''}
					{log.type === 'untrack' ? 'bg-pink-500/20 text-pink-300' : ''}
					{log.type === 'mount' ? 'bg-cyan-500/20 text-cyan-300' : ''}
					{log.type === 'info' ? 'bg-gray-500/20 text-gray-300' : ''}"
				>
					<span class="text-xs opacity-70">[{log.timestamp}]</span>
					<span class="ml-2">{log.message}</span>
				</div>
			{/each}

			{#if effectLogs.length === 0}
				<p class="text-gray-500 text-center py-8">
					No effects logged yet. Try changing some values!
				</p>
			{/if}
		</div>
	</div>

	<!-- Effect Types Explanation -->
	<div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
		<div class="bg-white/10 rounded-2xl p-6 border border-white/20">
			<h3 class="text-2xl font-semibold text-purple-300 mb-4">🔄 Effect Types</h3>

			<div class="space-y-3">
				<div class="bg-black/30 rounded-lg p-3">
					<h4 class="text-blue-300 font-semibold">$effect()</h4>
					<p class="text-gray-400 text-sm">Runs after DOM updates when dependencies change</p>
				</div>

				<div class="bg-black/30 rounded-lg p-3">
					<h4 class="text-yellow-300 font-semibold">$effect.pre()</h4>
					<p class="text-gray-400 text-sm">Runs before DOM updates, useful for measurements</p>
				</div>

				<div class="bg-black/30 rounded-lg p-3">
					<h4 class="text-green-300 font-semibold">Cleanup Functions</h4>
					<p class="text-gray-400 text-sm">Return a function to clean up subscriptions/timers</p>
				</div>

				<div class="bg-black/30 rounded-lg p-3">
					<h4 class="text-pink-300 font-semibold">untrack()</h4>
					<p class="text-gray-400 text-sm">Access state without creating dependencies</p>
				</div>
			</div>
		</div>

		<div class="bg-white/10 rounded-2xl p-6 border border-white/20">
			<h3 class="text-2xl font-semibold text-orange-300 mb-4">⚠️ Best Practices</h3>

			<div class="space-y-3">
				<div class="bg-black/30 rounded-lg p-3">
					<h4 class="text-green-400 font-semibold">✅ Do</h4>
					<ul class="text-gray-400 text-sm mt-1 space-y-1">
						<li>• Use effects for side effects only</li>
						<li>• Return cleanup functions when needed</li>
						<li>• Use $effect.pre for DOM measurements</li>
					</ul>
				</div>

				<div class="bg-black/30 rounded-lg p-3">
					<h4 class="text-red-400 font-semibold">❌ Don't</h4>
					<ul class="text-gray-400 text-sm mt-1 space-y-1">
						<li>• Update state inside effects (use $derived instead)</li>
						<li>• Forget to clean up subscriptions</li>
						<li>• Create infinite loops</li>
					</ul>
				</div>
			</div>
		</div>
	</div>

	<!-- Conditional Rendering Demo -->
	{#if isVisible}
		<div class="bg-white/10 rounded-2xl p-6 border border-white/20 transition-all duration-300">
			<h3 class="text-2xl font-semibold text-cyan-300 mb-4">👁️ Conditional Component</h3>
			<p class="text-gray-300">
				This component is conditionally rendered. Watch the mount/unmount effects!
			</p>

			<div class="mt-4 p-4 bg-black/30 rounded-lg">
				<p class="text-white">Current count: {count}</p>
				<p class="text-white">Current name: {name || 'Not set'}</p>
				<p class="text-white">Timer: {_seconds} seconds</p>
			</div>
		</div>
	{/if}

	<!-- Code Examples -->
	<div class="bg-black/40 rounded-2xl p-6 border border-white/20">
		<h3 class="text-xl font-semibold text-yellow-300 mb-4">📝 Effect Examples</h3>
		<div class="grid grid-cols-1 lg:grid-cols-2 gap-4">
			<div>
				<p class="text-gray-400 mb-2">Basic effect with cleanup:</p>
				<pre class="bg-black/60 rounded-lg p-4 text-sm text-gray-300 overflow-x-auto"><code
						>$effect(() => {`{
  console.log('Count changed:', count);
  
  // Cleanup function
  return () => {
    console.log('Cleaning up...');
  };
}`});

// Pre-effect
$effect.pre(() => {`{
  console.log('Before DOM update');
}`});</code
					></pre>
			</div>
			<div>
				<p class="text-gray-400 mb-2">Effect with untrack:</p>
				<pre class="bg-black/60 rounded-lg p-4 text-sm text-gray-300 overflow-x-auto"><code
						>$effect(() => {`{
  // This creates a dependency on count
  if (count > 0) {
    // This doesn't create dependency on name
    untrack(() => {
      if (name) {
        console.log('Untracked access');
      }
    });
  }
}`});</code
					></pre>
			</div>
		</div>
	</div>
</div>
