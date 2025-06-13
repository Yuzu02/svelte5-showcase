<script lang="ts">
import { untrack } from 'svelte'

// Basic state
let counter = $state(0)
let name = $state('')

// Deep reactive state
let profile = $state({
	user: {
		name: 'Alice',
		preferences: {
			theme: 'dark',
			notifications: true
		}
	},
	posts: [
		{ id: 1, title: 'Hello Svelte 5!', likes: 5 },
		{ id: 2, title: 'Runes are awesome!', likes: 12 }
	]
})

// $state.raw for non-reactive state
let config = $state.raw({
	apiUrl: 'https://api.example.com',
	timeout: 5000
})

// State with snapshot
let history: Array<{ action: string; timestamp: string; data: unknown }> = $state([])

// Derived state
let totalLikes = $derived(profile.posts.reduce((sum, post) => sum + post.likes, 0))
let _profileSummary = $derived(
	`${profile.user.name} has ${profile.posts.length} posts with ${totalLikes} total likes`
)

// Complex derived state
let _stats = $derived.by(() => {
	const posts = profile.posts
	return {
		totalPosts: posts.length,
		averageLikes: posts.length > 0 ? Math.round(totalLikes / posts.length) : 0,
		mostLiked: posts.reduce(
			(max, post) => (post.likes > max.likes ? post : max),
			posts[0] || { likes: 0 }
		)
	}
})

// Functions
function incrementCounter() {
	counter++
}

function addPost() {
	const newPost = {
		id: Date.now(),
		title: `Post ${profile.posts.length + 1}`,
		likes: Math.floor(Math.random() * 20)
	}
	profile.posts.push(newPost)
}
function likePost(postId: number) {
	const post = profile.posts.find((p) => p.id === postId)
	if (post) {
		post.likes++
	}
}

function toggleTheme() {
	profile.user.preferences.theme = profile.user.preferences.theme === 'dark' ? 'light' : 'dark'
}

function saveSnapshot() {
	// Using $state.snapshot to capture current state
	const snapshot = $state.snapshot(profile)
	history.push({
		timestamp: new Date().toLocaleTimeString(),
		data: snapshot,
		action: ''
	})
}

function updateConfigWithoutReactivity() {
	// This won't trigger reactivity because config is $state.raw
	config.timeout = Math.random() * 10000
}

function demonstrateUntrack() {
	// Using untrack to prevent this from being a dependency
	untrack(() => {
		console.log('Current counter value (untracked):', counter)
	})
	// This will still trigger reactivity
	counter++
}
</script>

<div class="space-y-8">
	<div class="text-center">
		<h2 class="text-4xl font-bold text-white mb-4">🔄 State Management</h2>
		<p class="text-gray-300 text-lg">
			Explore advanced state management features: deep reactivity, snapshots, raw state, and
			untracking.
		</p>
	</div>

	<!-- Basic State Demo -->
	<div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
		<div class="bg-white/10 rounded-2xl p-6 border border-white/20">
			<h3 class="text-2xl font-semibold text-blue-300 mb-4">💎 Basic State</h3>
			<div class="space-y-4">
				<div class="bg-black/30 rounded-lg p-4">
					<p class="block text-white mb-2">Counter: {counter}</p>
					<button
						class="bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded-lg transition-colors"
						onclick={incrementCounter}
					>
						Increment
					</button>
				</div>

				<div class="bg-black/30 rounded-lg p-4">
					<label for="state-name" class="block text-white mb-2">Name:</label>
					<input
						id="state-name"
						class="w-full px-3 py-2 bg-black/50 border border-gray-600 rounded-lg text-white focus:border-blue-500 focus:outline-none"
						type="text"
						bind:value={name}
						placeholder="Enter your name"
					/>
					{#if name}
						<p class="text-green-400 mt-2">Hello, {name}! 👋</p>
					{/if}
				</div>
			</div>
		</div>

		<!-- Deep Reactive State -->
		<div class="bg-white/10 rounded-2xl p-6 border border-white/20">
			<h3 class="text-2xl font-semibold text-purple-300 mb-4">🏗️ Deep Reactive State</h3>

			<div class="space-y-4">
				<div class="bg-black/30 rounded-lg p-4">
					<p class="text-white mb-2">User Profile:</p>
					<p class="text-gray-300">Name: {profile.user.name}</p>
					<p class="text-gray-300">Theme: {profile.user.preferences.theme}</p>
					<button
						class="mt-2 bg-purple-500 hover:bg-purple-600 text-white px-4 py-2 rounded-lg transition-colors"
						onclick={toggleTheme}
					>
						Toggle Theme
					</button>
				</div>

				<div class="bg-black/30 rounded-lg p-4">
					<p class="text-white mb-2">Posts ({profile.posts.length}):</p>
					<div class="space-y-2 max-h-32 overflow-y-auto">
						{#each profile.posts as post}
							<div class="flex justify-between items-center bg-black/40 rounded p-2">
								<span class="text-gray-300 text-sm">{post.title}</span>
								<div class="flex items-center space-x-2">
									<span class="text-yellow-400">❤️ {post.likes}</span>
									<button
										class="bg-red-500 hover:bg-red-600 text-white px-2 py-1 rounded text-xs"
										onclick={() => likePost(post.id)}
									>
										Like
									</button>
								</div>
							</div>
						{/each}
					</div>
					<button
						class="mt-2 bg-green-500 hover:bg-green-600 text-white px-4 py-2 rounded-lg transition-colors"
						onclick={addPost}
					>
						Add Post
					</button>
				</div>
			</div>
		</div>
	</div>

	<!-- Derived State -->
	<div class="bg-white/10 rounded-2xl p-6 border border-white/20">
		<h3 class="text-2xl font-semibold text-green-300 mb-4">⚡ Derived State</h3>

		<div class="grid grid-cols-1 md:grid-cols-3 gap-4">
			<div class="bg-black/30 rounded-lg p-4">
				<p class="text-white font-semibold">Profile Summary</p>
				<p class="text-gray-300 text-sm mt-2">{_profileSummary}</p>
			</div>

			<div class="bg-black/30 rounded-lg p-4">
				<p class="text-white font-semibold">Statistics</p>
				<div class="text-gray-300 text-sm mt-2 space-y-1">
					<p>Total Posts: {_stats.totalPosts}</p>
					<p>Average Likes: {_stats.averageLikes}</p>
					<p>Most Liked: "{_stats.mostLiked?.title || 'None'}"</p>
				</div>
			</div>

			<div class="bg-black/30 rounded-lg p-4">
				<p class="text-white font-semibold">Total Engagement</p>
				<p class="text-green-400 text-2xl font-bold mt-2">{totalLikes}</p>
				<p class="text-gray-400 text-xs">likes across all posts</p>
			</div>
		</div>
	</div>

	<!-- Raw State and Snapshots -->
	<div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
		<div class="bg-white/10 rounded-2xl p-6 border border-white/20">
			<h3 class="text-2xl font-semibold text-orange-300 mb-4">🔒 $state.raw</h3>
			<p class="text-gray-400 mb-4">
				Non-reactive state that only triggers updates when reassigned.
			</p>

			<div class="bg-black/30 rounded-lg p-4">
				<p class="text-white mb-2">Config (Raw State):</p>
				<p class="text-gray-300 text-sm">API URL: {config.apiUrl}</p>
				<p class="text-gray-300 text-sm">Timeout: {config.timeout}ms</p>
				<button
					class="mt-2 bg-orange-500 hover:bg-orange-600 text-white px-4 py-2 rounded-lg transition-colors"
					onclick={updateConfigWithoutReactivity}
				>
					Update Timeout (No Reactivity)
				</button>
			</div>
		</div>

		<div class="bg-white/10 rounded-2xl p-6 border border-white/20">
			<h3 class="text-2xl font-semibold text-pink-300 mb-4">📸 State Snapshots</h3>
			<p class="text-gray-400 mb-4">
				Capture static snapshots of reactive state for history/undo functionality.
			</p>

			<div class="space-y-3">
				<button
					class="bg-pink-500 hover:bg-pink-600 text-white px-4 py-2 rounded-lg transition-colors"
					onclick={saveSnapshot}
				>
					Save Snapshot
				</button>

				<div class="bg-black/30 rounded-lg p-4 max-h-32 overflow-y-auto">
					<p class="text-white mb-2">History ({history.length} snapshots):</p>
					{#each history as snapshot}
						<div class="text-gray-300 text-xs border-b border-gray-700 pb-1 mb-1">
							{snapshot.timestamp}: {(snapshot.data as typeof profile).posts.length} posts, {(snapshot.data as typeof profile).user.name}
						</div>
					{/each}
				</div>
			</div>
		</div>
	</div>

	<!-- Untrack Demo -->
	<div class="bg-white/10 rounded-2xl p-6 border border-white/20">
		<h3 class="text-2xl font-semibold text-red-300 mb-4">🚫 Untrack Function</h3>
		<p class="text-gray-400 mb-4">Use untrack() to access state without creating dependencies.</p>

		<div class="bg-black/30 rounded-lg p-4">
			<p class="text-white mb-2">Counter: {counter}</p>
			<button
				class="bg-red-500 hover:bg-red-600 text-white px-4 py-2 rounded-lg transition-colors"
				onclick={demonstrateUntrack}
			>
				Increment + Untrack Demo
			</button>
			<p class="text-gray-400 text-sm mt-2">Check console for untracked access log</p>
		</div>
	</div>

	<!-- Code Examples -->
	<div class="bg-black/40 rounded-2xl p-6 border border-white/20">
		<h3 class="text-xl font-semibold text-yellow-300 mb-4">📝 State Management Examples</h3>
		<div class="grid grid-cols-1 lg:grid-cols-2 gap-4">
			<div>
				<p class="text-gray-400 mb-2">Deep reactive state:</p>
				<pre class="bg-black/60 rounded-lg p-4 text-sm text-gray-300 overflow-x-auto"><code
						>let profile = $state({`{
  user: {
    name: 'Alice',
    preferences: { theme: 'dark' }
  },
  posts: [{ id: 1, title: 'Hello!' }]
}`});

// All nested properties are reactive
profile.user.name = 'Bob';
profile.posts.push(newPost);</code
					></pre>
			</div>
			<div>
				<p class="text-gray-400 mb-2">Raw state and snapshots:</p>
				<pre class="bg-black/60 rounded-lg p-4 text-sm text-gray-300 overflow-x-auto"><code
						>let config = $state.raw({`{ timeout: 5000 }`});

// Create snapshot
const snapshot = $state.snapshot(profile);

// Untrack access
untrack(() => {`{
  console.log(counter); // No dependency
}`});</code
					></pre>
			</div>
		</div>
	</div>
</div>
