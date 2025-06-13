<script lang="ts">
import Card from './Card.svelte'

interface Item {
	id: number
	title: string
	description: string
	color: string
}

let _selectedCard = $state<Item | null>(null)
let _items = $state<Item[]>([
	{
		id: 1,
		title: 'Reactive Programming',
		description: 'Build dynamic UIs with reactive state',
		color: 'purple'
	},
	{
		id: 2,
		title: 'Component Composition',
		description: 'Reusable snippets for better architecture',
		color: 'blue'
	},
	{
		id: 3,
		title: 'Modern Syntax',
		description: 'Clean and intuitive template syntax',
		color: 'green'
	}
])

function selectCard(item: Item) {
	_selectedCard = item
}
</script>

<div class="space-y-8">
	<div class="text-center">
		<h2 class="text-4xl font-bold text-white mb-4">📄 Snippets System</h2>
		<p class="text-gray-300 text-lg">
			Snippets are reusable chunks of markup that replace slots. They're more flexible and powerful
			than before!
		</p>
	</div>

	<!-- Basic Snippets Demo -->
	<div class="bg-white/10 rounded-2xl p-6 border border-white/20">
		<h3 class="text-2xl font-semibold text-blue-300 mb-4">🎨 Basic Snippets</h3>
		{#snippet cardSnippet(item: Item, isSelected = false)}
			<button
				class="w-full text-left p-4 rounded-lg border-2 transition-all duration-300
					{isSelected
					? 'border-yellow-400 bg-yellow-400/20 scale-105'
					: 'border-gray-600 bg-white/5 hover:bg-white/10'}"
				onclick={() => selectCard(item)}
			>
				<div class="flex items-center space-x-3">
					<div class="w-4 h-4 rounded-full bg-{item.color}-500"></div>
					<h4 class="text-white font-semibold">{item.title}</h4>
				</div>
				<p class="text-gray-400 mt-2">{item.description}</p>
				{#if isSelected}
					<div class="mt-3 text-yellow-400 text-sm font-medium">✨ Selected!</div>
				{/if}
			</button>
		{/snippet}

		<div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-6">
			{#each _items as item}
				{@render cardSnippet(item, _selectedCard?.id === item.id)}
			{/each}
		</div>

		{#if _selectedCard}
			<div class="bg-black/30 rounded-lg p-4">
				<p class="text-white">
					Selected: <span class="text-yellow-400 font-semibold">{_selectedCard.title}</span>
				</p>
			</div>
		{/if}
	</div>

	<!-- Snippets with Parameters -->
	<div class="bg-white/10 rounded-2xl p-6 border border-white/20">
		<h3 class="text-2xl font-semibold text-green-300 mb-4">🔧 Snippets with Parameters</h3>

		{#snippet statusBadge(status: string, count = 0)}
			<span
				class="inline-flex items-center px-3 py-1 rounded-full text-sm font-medium
				{status === 'success' ? 'bg-green-500/20 text-green-400 border border-green-500/30' : ''}
				{status === 'warning' ? 'bg-yellow-500/20 text-yellow-400 border border-yellow-500/30' : ''}
				{status === 'error' ? 'bg-red-500/20 text-red-400 border border-red-500/30' : ''}
				{status === 'info' ? 'bg-blue-500/20 text-blue-400 border border-blue-500/30' : ''}"
			>
				{#if status === 'success'}✅{/if}
				{#if status === 'warning'}⚠️{/if}
				{#if status === 'error'}❌{/if}
				{#if status === 'info'}ℹ️{/if}
				{status.charAt(0).toUpperCase() + status.slice(1)}
				{#if count > 0}({count}){/if}
			</span>
		{/snippet}

		<div class="flex flex-wrap gap-3">
			{@render statusBadge('success', 12)}
			{@render statusBadge('warning', 3)}
			{@render statusBadge('error', 1)}
			{@render statusBadge('info')}
		</div>
	</div>

	<!-- Passing Snippets to Components -->
	<div class="bg-white/10 rounded-2xl p-6 border border-white/20">
		<h3 class="text-2xl font-semibold text-purple-300 mb-4">🎯 Passing Snippets to Components</h3>

		{#snippet customHeader()}
			<div class="flex items-center space-x-3">
				<div
					class="w-10 h-10 bg-gradient-to-r from-purple-500 to-pink-500 rounded-lg flex items-center justify-center"
				>
					<span class="text-white font-bold">🚀</span>
				</div>
				<div>
					<h4 class="text-white font-semibold">Custom Header Snippet</h4>
					<p class="text-gray-400 text-sm">Passed as a prop to the Card component</p>
				</div>
			</div>
		{/snippet}

		{#snippet customContent()}
			<div class="space-y-3">
				<p class="text-gray-300">This content comes from a snippet passed to the component!</p>
				<div class="flex space-x-2">
					<button
						class="bg-purple-500 hover:bg-purple-600 text-white px-4 py-2 rounded-lg transition-colors"
					>
						Action 1
					</button>
					<button
						class="bg-pink-500 hover:bg-pink-600 text-white px-4 py-2 rounded-lg transition-colors"
					>
						Action 2
					</button>
				</div>
			</div>
		{/snippet}

		<Card header={customHeader} content={customContent} />
	</div>

	<!-- Recursive Snippets -->
	<div class="bg-white/10 rounded-2xl p-6 border border-white/20">
		<h3 class="text-2xl font-semibold text-orange-300 mb-4">🔄 Recursive Snippets</h3>

		{#snippet treeNode(node: any, depth = 0)}
			<div class="ml-{depth * 4}">
				<div class="flex items-center space-x-2 py-1">
					<span class="text-{['red', 'green', 'blue', 'purple', 'yellow'][depth % 5]}-400">
						{'→'.repeat(depth + 1)}
					</span>
					<span class="text-white">{node.name}</span>
				</div>
				{#if node.children}
					{#each node.children as child}
						{@render treeNode(child, depth + 1)}
					{/each}
				{/if}
			</div>
		{/snippet}

		{@render treeNode({
			name: 'Root',
			children: [
				{
					name: 'Branch 1',
					children: [{ name: 'Leaf 1.1' }, { name: 'Leaf 1.2' }]
				},
				{
					name: 'Branch 2',
					children: [
						{
							name: 'Sub-branch 2.1',
							children: [{ name: 'Deep Leaf 2.1.1' }]
						}
					]
				}
			]
		})}
	</div>

	<!-- Code Examples -->
	<div class="bg-black/40 rounded-2xl p-6 border border-white/20">
		<h3 class="text-xl font-semibold text-yellow-300 mb-4">📝 Snippet Syntax Examples</h3>
		<div class="grid grid-cols-1 lg:grid-cols-2 gap-4">
			<div>
				<p class="text-gray-400 mb-2">Basic snippet:</p>
				<pre class="bg-black/60 rounded-lg p-4 text-sm text-gray-300 overflow-x-auto"><code
						>{`{#snippet cardSnippet(item, isSelected = false)}`}
  &lt;div class="card"&gt;
    &lt;h4&gt;{`{item.title}`}&lt;/h4&gt;
  &lt;/div&gt;
{`{/snippet}`}

{`{@render cardSnippet(item, true)}`}</code
					></pre>
			</div>
			<div>
				<p class="text-gray-400 mb-2">Passing to component:</p>
				<pre class="bg-black/60 rounded-lg p-4 text-sm text-gray-300 overflow-x-auto"><code
						>{`{#snippet myHeader()}`}
  &lt;h1&gt;Custom Header&lt;/h1&gt;
{`{/snippet}`}

&lt;Card header={`{myHeader}`} /&gt;</code
					></pre>
			</div>
		</div>
	</div>
</div>
