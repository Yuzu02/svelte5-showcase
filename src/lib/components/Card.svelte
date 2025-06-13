<script lang="ts">
import type { Snippet } from 'svelte'

interface Props {
	header?: Snippet
	content?: Snippet
	footer?: Snippet
}

let { header, content, footer }: Props = $props()
</script>

<div
	class="bg-gradient-to-r from-purple-500/10 to-pink-500/10 rounded-xl border border-purple-500/30 overflow-hidden"
>
	{#if header}
		<div class="p-4 border-b border-purple-500/20">
			{@render header()}
		</div>
	{/if}

	{#if content}
		<div class="p-4">
			{@render content()}
		</div>
	{/if}

	{#if footer}
		<div class="p-4 border-t border-purple-500/20 bg-black/20">
			{@render footer()}
		</div>
	{/if}
</div>
