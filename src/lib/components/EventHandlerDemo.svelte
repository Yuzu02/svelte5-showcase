<script lang="ts">
import { createEventDispatcher } from 'svelte'

let clickCount = $state(0)
let _lastEvent: { type: string; target: string; clientX: number; clientY: number } | null =
	$state(null)
let _isPressed = $state(false)
let _mousePosition = $state({ x: 0, y: 0 })
let _keyPressed = $state('')
let formData = $state({ name: '', email: '', message: '' })
let eventLog: Array<{
	id: number
	type: string
	timestamp: string
	data: Record<string, unknown>
}> = $state([])

const dispatch = createEventDispatcher()

function logEvent(eventType: string, data = {}) {
	eventLog.unshift({
		id: Date.now(),
		type: eventType,
		data,
		timestamp: new Date().toLocaleTimeString()
	})

	// Keep only last 15 events
	if (eventLog.length > 15) {
		eventLog.pop()
	}
}
// Event handlers using new syntax
function handleClick(event: MouseEvent) {
	clickCount++
	_lastEvent = {
		type: 'click',
		target: (event.target as HTMLElement).tagName,
		clientX: event.clientX,
		clientY: event.clientY
	}
	logEvent('click', { count: clickCount, x: event.clientX, y: event.clientY })
}

function handleDoubleClick(event: MouseEvent) {
	logEvent('doubleclick', { target: (event.target as HTMLElement).textContent })
}

function handleMouseMove(event: MouseEvent) {
	_mousePosition = { x: event.clientX, y: event.clientY }
}

function handleMouseDown() {
	_isPressed = true
	logEvent('mousedown')
}

function handleMouseUp() {
	_isPressed = false
	logEvent('mouseup')
}

function handleKeyDown(event: KeyboardEvent) {
	_keyPressed = event.key
	logEvent('keydown', { key: event.key, code: event.code })
}

function handleKeyUp() {
	_keyPressed = ''
	logEvent('keyup')
}

function handleFormSubmit(event: SubmitEvent) {
	event.preventDefault()
	logEvent('form-submit', { ...formData })
	dispatch('formSubmitted', formData)
}

function handleInput(event: Event, field: string) {
	const target = event.target as HTMLInputElement | HTMLTextAreaElement
	;(formData as Record<string, string>)[field] = target.value
	logEvent('input', { field, value: target.value })
}

function handleCustomEvent() {
	const customData = {
		timestamp: Date.now(),
		random: Math.random(),
		message: 'Custom event triggered!'
	}
	dispatch('customEvent', customData)
	logEvent('custom-event', customData)
}

function clearLog() {
	eventLog.length = 0
}
function handleContextMenu(event: MouseEvent) {
	event.preventDefault()
	logEvent('context-menu', { x: event.clientX, y: event.clientY })
}

function handleFocus(field: string) {
	logEvent('focus', { field })
}

function handleBlur(field: string) {
	logEvent('blur', { field })
}
</script>

<svelte:window onkeydown={handleKeyDown} onkeyup={handleKeyUp} />
<svelte:document on:mousemove={handleMouseMove} />

<div class="space-y-8">
	<div class="text-center">
		<h2 class="text-4xl font-bold text-white mb-4">🎯 Event Handlers</h2>
		<p class="text-gray-300 text-lg">
			Explore modern event handling in Svelte 5: inline handlers, event delegation, and custom
			events.
		</p>
	</div>

	<!-- Status Bar -->
	<div class="bg-white/10 rounded-2xl p-4 border border-white/20">
		<div class="grid grid-cols-2 md:grid-cols-4 gap-4 text-center">
			<div>
				<p class="text-gray-400 text-sm">Clicks</p>
				<p class="text-white text-2xl font-bold">{clickCount}</p>
			</div>
			<div>
				<p class="text-gray-400 text-sm">Mouse Position</p>
				<p class="text-white text-lg">{_mousePosition.x}, {_mousePosition.y}</p>
			</div>
			<div>
				<p class="text-gray-400 text-sm">Key Pressed</p>
				<p class="text-white text-lg font-mono">{_keyPressed || 'None'}</p>
			</div>
			<div>
				<p class="text-gray-400 text-sm">Mouse State</p>
				<p class="text-white text-lg">{_isPressed ? '🔽 Pressed' : '🔼 Released'}</p>
			</div>
		</div>
	</div>

	<!-- Interactive Elements -->
	<div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
		<!-- Click Handlers -->
		<div class="bg-white/10 rounded-2xl p-6 border border-white/20">
			<h3 class="text-2xl font-semibold text-blue-300 mb-4">🖱️ Click Events</h3>

			<div class="space-y-4">
				<button
					class="w-full bg-blue-500 hover:bg-blue-600 text-white py-4 rounded-lg transition-all duration-200 transform hover:scale-105"
					onclick={handleClick}
				>
					Click Me! (Count: {clickCount})
				</button>

				<button
					class="w-full bg-purple-500 hover:bg-purple-600 text-white py-4 rounded-lg transition-colors"
					ondblclick={handleDoubleClick}
				>
					Double Click Me!
				</button>
				<div
					class="w-full h-20 bg-gradient-to-r from-green-500 to-blue-500 rounded-lg flex items-center justify-center text-white cursor-pointer transition-all duration-200 {_isPressed
						? 'scale-95'
						: ''}"
					role="button"
					tabindex="0"
					onmousedown={handleMouseDown}
					onmouseup={handleMouseUp}
					oncontextmenu={handleContextMenu}
				>
					Press and Hold (Right-click too!)
				</div>
			</div>
		</div>

		<!-- Form Events -->
		<div class="bg-white/10 rounded-2xl p-6 border border-white/20">
			<h3 class="text-2xl font-semibold text-green-300 mb-4">📝 Form Events</h3>
			<form onsubmit={handleFormSubmit} class="space-y-4">
				<div>
					<label for="form-name" class="block text-white mb-2">Name:</label>
					<input
						id="form-name"
						class="w-full px-3 py-2 bg-black/50 border border-gray-600 rounded-lg text-white focus:border-green-500 focus:outline-none transition-colors"
						type="text"
						value={formData.name}
						oninput={(e) => handleInput(e, 'name')}
						onfocus={() => handleFocus('name')}
						onblur={() => handleBlur('name')}
						placeholder="Enter your name"
					/>
				</div>

				<div>
					<label for="form-email" class="block text-white mb-2">Email:</label>
					<input
						id="form-email"
						class="w-full px-3 py-2 bg-black/50 border border-gray-600 rounded-lg text-white focus:border-green-500 focus:outline-none transition-colors"
						type="email"
						value={formData.email}
						oninput={(e) => handleInput(e, 'email')}
						onfocus={() => handleFocus('email')}
						onblur={() => handleBlur('email')}
						placeholder="Enter your email"
					/>
				</div>

				<div>
					<label for="form-message" class="block text-white mb-2">Message:</label>
					<textarea
						id="form-message"
						class="w-full px-3 py-2 bg-black/50 border border-gray-600 rounded-lg text-white focus:border-green-500 focus:outline-none transition-colors resize-none"
						rows="3"
						value={formData.message}
						oninput={(e) => handleInput(e, 'message')}
						onfocus={() => handleFocus('message')}
						onblur={() => handleBlur('message')}
						placeholder="Enter your message"
					></textarea>
				</div>

				<button
					type="submit"
					class="w-full bg-green-500 hover:bg-green-600 text-white py-3 rounded-lg transition-colors"
				>
					Submit Form
				</button>
			</form>
		</div>
	</div>

	<!-- Custom Events -->
	<div class="bg-white/10 rounded-2xl p-6 border border-white/20">
		<h3 class="text-2xl font-semibold text-purple-300 mb-4">🎨 Custom Events</h3>

		<div class="grid grid-cols-1 md:grid-cols-3 gap-4">
			<button
				class="bg-purple-500 hover:bg-purple-600 text-white py-3 rounded-lg transition-colors"
				onclick={handleCustomEvent}
			>
				Trigger Custom Event
			</button>

			<button
				class="bg-orange-500 hover:bg-orange-600 text-white py-3 rounded-lg transition-colors"
				onclick={() => dispatch('notification', { type: 'success', message: 'Success!' })}
			>
				Success Notification
			</button>

			<button
				class="bg-red-500 hover:bg-red-600 text-white py-3 rounded-lg transition-colors"
				onclick={() => dispatch('notification', { type: 'error', message: 'Error occurred!' })}
			>
				Error Notification
			</button>
		</div>
	</div>

	<!-- Event Log -->
	<div class="bg-white/10 rounded-2xl p-6 border border-white/20">
		<div class="flex justify-between items-center mb-4">
			<h3 class="text-2xl font-semibold text-yellow-300">📋 Event Log</h3>
			<button
				class="bg-gray-500 hover:bg-gray-600 text-white px-4 py-2 rounded-lg transition-colors"
				onclick={clearLog}
			>
				Clear Log
			</button>
		</div>

		<div class="bg-black/40 rounded-lg p-4 h-64 overflow-y-auto border border-gray-600">
			{#each eventLog as log}
				<div
					class="mb-2 p-2 rounded text-sm border-l-4
					{log.type === 'click' ? 'border-blue-500 bg-blue-500/10' : ''}
					{log.type === 'doubleclick' ? 'border-purple-500 bg-purple-500/10' : ''}
					{log.type === 'keydown' ? 'border-green-500 bg-green-500/10' : ''}
					{log.type === 'input' ? 'border-yellow-500 bg-yellow-500/10' : ''}
					{log.type === 'form-submit' ? 'border-emerald-500 bg-emerald-500/10' : ''}
					{log.type === 'custom-event' ? 'border-pink-500 bg-pink-500/10' : ''}
					{log.type.includes('mouse') ? 'border-orange-500 bg-orange-500/10' : ''}
					{log.type.includes('focus') || log.type.includes('blur') ? 'border-cyan-500 bg-cyan-500/10' : ''}"
				>
					<div class="flex justify-between items-start">
						<div>
							<span class="font-semibold text-white capitalize">{log.type.replace('-', ' ')}</span>
							{#if log.data && Object.keys(log.data).length > 0}
								<div class="text-gray-300 text-xs mt-1">
									{JSON.stringify(log.data, null, 1).slice(0, 100)}
									{JSON.stringify(log.data).length > 100 ? '...' : ''}
								</div>
							{/if}
						</div>
						<span class="text-xs text-gray-400">{log.timestamp}</span>
					</div>
				</div>
			{/each}

			{#if eventLog.length === 0}
				<p class="text-gray-500 text-center py-8">
					No events logged yet. Try interacting with the elements!
				</p>
			{/if}
		</div>
	</div>

	<!-- Event Handler Syntax -->
	<div class="bg-black/40 rounded-2xl p-6 border border-white/20">
		<h3 class="text-xl font-semibold text-yellow-300 mb-4">📝 Event Handler Syntax</h3>
		<div class="grid grid-cols-1 lg:grid-cols-2 gap-4">
			<div>
				<p class="text-gray-400 mb-2">Modern inline handlers:</p>
				<pre class="bg-black/60 rounded-lg p-4 text-sm text-gray-300 overflow-x-auto"><code
						>&lt;button onclick={`{() => count++}`}&gt;
  Increment
&lt;/button&gt;

&lt;input 
  oninput={`{(e) => name = e.target.value}`}
  value={`{name}`}
/&gt;

&lt;div onmousedown={`{handleMouseDown}`}&gt;
  Press me
&lt;/div&gt;</code
					></pre>
			</div>
			<div>
				<p class="text-gray-400 mb-2">Custom events:</p>
				<pre class="bg-black/60 rounded-lg p-4 text-sm text-gray-300 overflow-x-auto"><code
						>import {`{ createEventDispatcher }`} from 'svelte';

const dispatch = createEventDispatcher();

function handleClick() {`{
  dispatch('customEvent', {
    message: 'Hello!',
    timestamp: Date.now()
  });
}`}</code
					></pre>
			</div>
		</div>
	</div>

	<!-- Last Event Details -->
	{#if _lastEvent}
		<div class="bg-white/10 rounded-2xl p-6 border border-white/20">
			<h3 class="text-2xl font-semibold text-red-300 mb-4">🔍 Last Click Event Details</h3>
			<div class="bg-black/30 rounded-lg p-4">
				<p class="text-white">Type: <span class="text-blue-400">{_lastEvent.type}</span></p>
				<p class="text-white">Target: <span class="text-green-400">{_lastEvent.target}</span></p>
				<p class="text-white">
					Position: <span class="text-yellow-400">
						{#if _lastEvent instanceof MouseEvent}
							({_lastEvent.clientX}, {_lastEvent.clientY})
						{:else}
							N/A
						{/if}
					</span>
				</p>
			</div>
		</div>
	{/if}
</div>
