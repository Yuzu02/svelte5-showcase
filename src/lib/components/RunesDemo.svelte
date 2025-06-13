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

<div class="space-y-6 sm:space-y-8">
    <div class="text-center">
        <h2
            class="text-2xl sm:text-3xl md:text-4xl font-bold text-white mb-3 sm:mb-4"
        >
            🪄 Runes System
        </h2>
        <p class="text-gray-300 text-base sm:text-lg px-2">
            Runes are the new reactive primitives in Svelte 5. They replace the
            old reactive statements and provide more explicit control.
        </p>
    </div>

    <div class="grid grid-cols-1 xl:grid-cols-2 gap-4 sm:gap-6 lg:gap-8">
        <!-- $state Demo -->
        <div
            class="bg-white/10 rounded-xl sm:rounded-2xl p-4 sm:p-6 border border-white/20"
        >
            <h3
                class="text-xl sm:text-2xl font-semibold text-purple-300 mb-3 sm:mb-4"
            >
                💎 $state Rune
            </h3>
            <p class="text-gray-400 mb-3 sm:mb-4 text-sm sm:text-base">
                Creates reactive state that triggers UI updates when changed.
            </p>

            <div class="space-y-3 sm:space-y-4">
                <div
                    class="flex flex-col sm:flex-row items-start sm:items-center justify-between bg-black/30 rounded-lg p-3 sm:p-4 gap-3 sm:gap-0"
                >
                    <span class="text-white font-mono text-sm sm:text-base"
                        >count = {count}</span
                    >
                    <div class="flex space-x-2">
                        <button
                            class="bg-red-500 hover:bg-red-600 text-white px-3 py-2 sm:px-4 rounded-lg transition-colors text-sm sm:text-base"
                            onclick={decrement}
                        >
                            -
                        </button>
                        <button
                            class="bg-green-500 hover:bg-green-600 text-white px-3 py-2 sm:px-4 rounded-lg transition-colors text-sm sm:text-base"
                            onclick={increment}
                        >
                            +
                        </button>
                    </div>
                </div>

                <div class="bg-black/30 rounded-lg p-3 sm:p-4">
                    <p class="text-white mb-2 text-sm sm:text-base">
                        User Object (deeply reactive):
                    </p>
                    <p class="text-gray-300 font-mono text-sm sm:text-base">
                        {_userInfo}
                    </p>
                    <button
                        class="mt-2 bg-blue-500 hover:bg-blue-600 text-white px-3 py-2 sm:px-4 rounded-lg transition-colors text-sm sm:text-base"
                        onclick={updateUserAge}
                    >
                        Age Up
                    </button>
                </div>

                <div class="bg-black/30 rounded-lg p-3 sm:p-4">
                    <p class="text-white mb-2 text-sm sm:text-base">
                        Items Array ({_itemCount} items):
                    </p>
                    <div class="flex flex-wrap gap-2 mb-3">
                        {#each items as item}
                            <span
                                class="bg-purple-500 text-white px-2 py-1 sm:px-3 rounded-full text-xs sm:text-sm"
                                >{item}</span
                            >
                        {/each}
                    </div>
                    <div
                        class="flex flex-col sm:flex-row gap-2 sm:space-x-2 sm:gap-0"
                    >
                        <button
                            class="bg-green-500 hover:bg-green-600 text-white px-3 py-2 sm:px-4 rounded-lg transition-colors text-sm sm:text-base"
                            onclick={addRandomItem}
                        >
                            Add Item
                        </button>
                        <button
                            class="bg-red-500 hover:bg-red-600 text-white px-3 py-2 sm:px-4 rounded-lg transition-colors text-sm sm:text-base"
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
        <div
            class="bg-white/10 rounded-xl sm:rounded-2xl p-4 sm:p-6 border border-white/20"
        >
            <h3
                class="text-xl sm:text-2xl font-semibold text-green-300 mb-3 sm:mb-4"
            >
                ⚡ $derived Rune
            </h3>
            <p class="text-gray-400 mb-3 sm:mb-4 text-sm sm:text-base">
                Creates computed values that automatically update when
                dependencies change.
            </p>

            <div class="space-y-3 sm:space-y-4">
                <div class="bg-black/30 rounded-lg p-3 sm:p-4">
                    <p class="text-white text-sm sm:text-base">
                        Simple derived values:
                    </p>
                    <div class="mt-2 space-y-1">
                        <p class="text-gray-300 font-mono text-xs sm:text-sm">
                            doubled = {_doubled}
                        </p>
                        <p class="text-gray-300 font-mono text-xs sm:text-sm">
                            isEven = {_isEven}
                        </p>
                    </div>
                </div>
                <div class="bg-black/30 rounded-lg p-3 sm:p-4">
                    <p class="text-white text-sm sm:text-base">
                        Complex derived with $derived.by():
                    </p>
                    <p
                        class="text-gray-300 font-mono mt-2 text-xs sm:text-sm break-all"
                    >
                        expensiveResult = {_expensiveComputation.toFixed(2)}
                    </p>
                    <p class="text-xs text-gray-500 mt-1">
                        Check console for computation logs
                    </p>
                </div>

                <div class="bg-black/30 rounded-lg p-3 sm:p-4">
                    <p class="text-white text-sm sm:text-base">
                        String interpolation derived:
                    </p>
                    <p
                        class="text-gray-300 font-mono mt-2 text-xs sm:text-sm break-all"
                    >
                        {_userInfo}
                    </p>
                </div>
            </div>
        </div>
    </div>

    <!-- Code Examples -->
    <div
        class="bg-black/40 rounded-xl sm:rounded-2xl p-4 sm:p-6 border border-white/20"
    >
        <h3
            class="text-lg sm:text-xl font-semibold text-yellow-300 mb-3 sm:mb-4"
        >
            📝 Code Examples
        </h3>
        <div class="grid grid-cols-1 lg:grid-cols-2 gap-3 sm:gap-4">
            <div>
                <p class="text-gray-400 mb-2 text-sm sm:text-base">
                    $state usage:
                </p>
                <pre
                    class="bg-black/60 rounded-lg p-3 sm:p-4 text-xs sm:text-sm text-gray-300 overflow-x-auto"><code
                        >let count = $state(0);
let user = $state({`{ name: 'John', age: 30 }`});
let items = $state(['Apple', 'Banana']);</code
                    ></pre>
            </div>
            <div>
                <p class="text-gray-400 mb-2 text-sm sm:text-base">
                    $derived usage:
                </p>
                <pre
                    class="bg-black/60 rounded-lg p-3 sm:p-4 text-xs sm:text-sm text-gray-300 overflow-x-auto"><code
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
