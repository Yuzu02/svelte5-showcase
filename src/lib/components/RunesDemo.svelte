<script>
// Demonstrating $state rune
let count = $state(0)
let user = $state({ name: 'John', age: 30 })
let items = $state(['Apple', 'Banana', 'Cherry'])

// Demonstrating $derived rune
let _doubled = $derived(count * 2)
let _isEven = $derived(count % 2 === 0)
let _userInfo = $derived(`${user.name} is ${user.age} years old`)
let _itemCount = $derived(items.length)
// Demonstrating $derived.by for complex derivations
let _expensiveComputation = $derived.by(() => {
	console.log('Computing expensive result...')
	// Use only the count for computation, no side effects like Math.random()
	return count ** 2 + count * 10
})

// Functions to modify state
function increment() {
	count++
}

function decrement() {
	count--
}

function addRandomItem() {
	const fruits = ['Orange', 'Grape', 'Mango', 'Pineapple', 'Kiwi']
	const randomFruit = fruits[Math.floor(Math.random() * fruits.length)]
	items.push(randomFruit)
}

function removeLastItem() {
	items.pop()
}

function updateUserAge() {
	user.age++
}
</script>

<div class="space-y-8">
	<div class="text-center">
		<h2 class="text-4xl font-bold text-white mb-4">🪄 Runes System</h2>
		<p class="text-gray-300 text-lg">
			Runes are the new reactive primitives in Svelte 5. They replace the old reactive statements
			and provide more explicit control.
		</p>
	</div>

	<div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
		<!-- $state Demo -->
		<div class="bg-white/10 rounded-2xl p-6 border border-white/20">
			<h3 class="text-2xl font-semibold text-purple-300 mb-4">💎 $state Rune</h3>
			<p class="text-gray-400 mb-4">
				Creates reactive state that triggers UI updates when changed.
			</p>

			<div class="space-y-4">
				<div class="flex items-center justify-between bg-black/30 rounded-lg p-4">
					<span class="text-white font-mono">count = {count}</span>
					<div class="flex space-x-2">
						<button
							class="bg-red-500 hover:bg-red-600 text-white px-4 py-2 rounded-lg transition-colors"
							onclick={decrement}
						>
							-
						</button>
						<button
							class="bg-green-500 hover:bg-green-600 text-white px-4 py-2 rounded-lg transition-colors"
							onclick={increment}
						>
							+
						</button>
					</div>
				</div>

				<div class="bg-black/30 rounded-lg p-4">
					<p class="text-white mb-2">User Object (deeply reactive):</p>
					<p class="text-gray-300 font-mono">{_userInfo}</p>
					<button
						class="mt-2 bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded-lg transition-colors"
						onclick={updateUserAge}
					>
						Age Up
					</button>
				</div>

				<div class="bg-black/30 rounded-lg p-4">
					<p class="text-white mb-2">Items Array ({_itemCount} items):</p>
					<div class="flex flex-wrap gap-2 mb-3">
						{#each items as item}
							<span class="bg-purple-500 text-white px-3 py-1 rounded-full text-sm">{item}</span>
						{/each}
					</div>
					<div class="flex space-x-2">
						<button
							class="bg-green-500 hover:bg-green-600 text-white px-4 py-2 rounded-lg transition-colors"
							onclick={addRandomItem}
						>
							Add Item
						</button>
						<button
							class="bg-red-500 hover:bg-red-600 text-white px-4 py-2 rounded-lg transition-colors"
							onclick={removeLastItem}
							disabled={items.length === 0}
						>
							Remove Last
						</button>
					</div>
				</div>
			</div>
		</div>

		<!-- $derived Demo -->
		<div class="bg-white/10 rounded-2xl p-6 border border-white/20">
			<h3 class="text-2xl font-semibold text-green-300 mb-4">⚡ $derived Rune</h3>
			<p class="text-gray-400 mb-4">
				Creates computed values that automatically update when dependencies change.
			</p>

			<div class="space-y-4">
				<div class="bg-black/30 rounded-lg p-4">
					<p class="text-white">Simple derived values:</p>
					<div class="mt-2 space-y-1">
						<p class="text-gray-300 font-mono">doubled = {_doubled}</p>
						<p class="text-gray-300 font-mono">isEven = {_isEven}</p>
					</div>
				</div>

				<div class="bg-black/30 rounded-lg p-4">
					<p class="text-white">Complex derived with $derived.by():</p>
					<p class="text-gray-300 font-mono mt-2">
						expensiveResult = {_expensiveComputation.toFixed(2)}
					</p>
					<p class="text-xs text-gray-500 mt-1">Check console for computation logs</p>
				</div>

				<div class="bg-black/30 rounded-lg p-4">
					<p class="text-white">String interpolation derived:</p>
					<p class="text-gray-300 font-mono mt-2">{_userInfo}</p>
				</div>
			</div>
		</div>
	</div>

	<!-- Code Examples -->
	<div class="bg-black/40 rounded-2xl p-6 border border-white/20">
		<h3 class="text-xl font-semibold text-yellow-300 mb-4">📝 Code Examples</h3>
		<div class="grid grid-cols-1 md:grid-cols-2 gap-4">
			<div>
				<p class="text-gray-400 mb-2">$state usage:</p>
				<pre class="bg-black/60 rounded-lg p-4 text-sm text-gray-300 overflow-x-auto"><code
						>let count = $state(0);
let user = $state({`{ name: 'John', age: 30 }`});
let items = $state(['Apple', 'Banana']);</code
					></pre>
			</div>
			<div>
				<p class="text-gray-400 mb-2">$derived usage:</p>
				<pre class="bg-black/60 rounded-lg p-4 text-sm text-gray-300 overflow-x-auto"><code
						>let doubled = $derived(count * 2);
let isEven = $derived(count % 2 === 0);
let complex = $derived.by(() => {`{
  return heavyComputation();
}`});</code
					></pre>
			</div>
		</div>
	</div>
</div>
