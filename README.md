# 🚀 Svelte 5 Showcase

A comprehensive demonstration of Svelte 5's revolutionary new features, built with modern tooling and beautiful UI. This project showcases the complete Svelte 5 ecosystem including Runes, Snippets, Enhanced State Management, Effects System, and Modern Event Handling.

![Svelte 5 Showcase](https://img.shields.io/badge/Svelte-5-ff3e00?style=for-the-badge&logo=svelte&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)
![Bun](https://img.shields.io/badge/Bun-000000?style=for-the-badge&logo=bun&logoColor=white)
![TailwindCSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white)

## ✨ Features Demonstrated

### 🪄 Runes System

- **$state rune**: Creates reactive state that triggers UI updates when changed
- **$derived rune**: Computed values that automatically update when dependencies change
- **$derived.by()**: Complex derivations with function bodies for expensive computations
- **Deep reactivity**: Objects and arrays become deeply reactive proxies
- **State snapshots**: Static snapshots of reactive state for history/undo functionality

### 📄 Snippets System

- **Reusable markup**: Create reusable chunks of markup with parameters
- **Component composition**: Pass snippets as props to components (replacing slots)
- **Recursive snippets**: Self-referencing snippets for tree structures
- **Optional snippets**: Optional chaining syntax for conditional snippet rendering
- **Typed snippets**: Full TypeScript support with `Snippet<[ParamTypes]>` interface

### 🔄 Advanced State Management

- **$state.raw**: Non-deeply reactive state for performance optimization
- **$state.snapshot**: Capture static snapshots for history tracking
- **Deep reactivity**: Nested objects and arrays are fully reactive
- **Untrack function**: Access state without creating dependencies
- **State ownership**: Proper mutation patterns and ownership validation

### ⚡ Effects System

- **$effect()**: Runs after DOM updates when dependencies change
- **$effect.pre()**: Runs before DOM updates, useful for measurements
- **Cleanup functions**: Automatic cleanup for subscriptions and timers
- **Dependency tracking**: Automatic tracking of reactive dependencies
- **Untrack access**: Use `untrack()` to access state without creating dependencies

### 🎯 Modern Event Handling

- **New syntax**: Modern `onclick` syntax replacing `on:click`
- **Event delegation**: Efficient event handling patterns
- **Custom events**: Component communication with custom event dispatching
- **Form handling**: Modern form submission and validation patterns
- **Accessibility**: ARIA labels and proper semantic elements

## 🛠️ Tech Stack

- **[Svelte 5](https://svelte.dev/)**: The reactive UI framework with Runes
- **[SvelteKit](https://kit.svelte.dev/)**: Full-stack web framework
- **[TypeScript](https://www.typescriptlang.org/)**: Type-safe JavaScript
- **[Bun](https://bun.sh/)**: Fast all-in-one JavaScript runtime & toolkit
- **[Tailwind CSS](https://tailwindcss.com/)**: Utility-first CSS framework
- **[Vite](https://vitejs.dev/)**: Next generation frontend tooling

## 🚀 Quick Start

### Prerequisites

- [Bun](https://bun.sh/) installed on your system

### Installation

1. **Clone the repository**

   ```bash
   git clone <repository-url>
   cd svelte5-showcase
   ```

2. **Install dependencies**

   ```bash
   bun install
   ```

3. **Start the development server**

   ```bash
   bun run dev
   ```

4. **Open your browser**
   Navigate to `http://localhost:5173` to see the showcase in action.

### Available Scripts

```bash
# Development
bun run dev          # Start development server with hot reload
bun run build        # Build for production
bun run preview      # Preview production build

# Quality Assurance
bun run check        # Run type checking
bun run check:watch  # Run type checking in watch mode
bun run lint         # Run linter
bun run format       # Format code with Prettier
```

## 📚 Project Structure

```
svelte5-showcase/
├── src/
│   ├── lib/
│   │   ├── components/
│   │   │   ├── Card.svelte           # Reusable card component with snippets
│   │   │   ├── RunesDemo.svelte      # $state and $derived demonstrations
│   │   │   ├── SnippetsDemo.svelte   # Snippets system showcase
│   │   │   ├── StateDemo.svelte      # Advanced state management
│   │   │   ├── EffectsDemo.svelte    # Effects system examples
│   │   │   └── EventHandlerDemo.svelte # Modern event handling
│   │   └── index.ts                  # Library exports
│   ├── routes/
│   │   ├── +layout.svelte           # Root layout
│   │   └── +page.svelte             # Main showcase page
│   ├── app.css                      # Global styles
│   ├── app.d.ts                     # TypeScript declarations
│   └── app.html                     # HTML template
├── static/                          # Static assets
├── biome.jsonc                      # Biome configuration
├── package.json                     # Dependencies and scripts
├── svelte.config.js                 # Svelte configuration
├── tailwind.config.js               # Tailwind CSS configuration
├── tsconfig.json                    # TypeScript configuration
└── vite.config.ts                   # Vite configuration
```

## 🎨 Component Showcase

### Runes Demo

Demonstrates the core reactive primitives:

- Interactive counter with `$state`
- User profile with deeply reactive objects
- Dynamic array manipulation
- Complex computed values with `$derived.by()`

### Snippets Demo

Shows the powerful new templating system:

- Basic snippets with parameters
- Snippets passed as component props
- Recursive tree structure rendering
- Status badges with conditional styling

### State Management Demo

Advanced state patterns:

- Raw state for non-reactive data
- State snapshots for history tracking
- Untrack function demonstrations  
- Deep reactivity examples

### Effects Demo

Side effect management:

- Basic effects with cleanup
- Pre-effects for DOM measurements
- Timer management with proper cleanup
- Conditional effect execution

### Event Handler Demo

Modern event handling patterns:

- Click and double-click handlers
- Mouse movement tracking
- Keyboard event handling
- Form submission patterns  
- Custom event dispatching

## 🔧 Development Patterns

### Runes Best Practices

```typescript
// ✅ Good: Pure derived values
let doubled = $derived(count * 2);
let isEven = $derived(count % 2 === 0);

// ✅ Good: Complex computations with $derived.by()
let stats = $derived.by(() => {
  return heavyComputation(data);
});

// ❌ Avoid: Side effects in derived
let bad = $derived(count * Math.random()); // Don't do this!
```

### State Management Patterns

```typescript
// ✅ Good: Deep reactive state
let profile = $state({
  user: { name: 'Alice', preferences: { theme: 'dark' } }
});

// ✅ Good: Raw state for non-reactive data
let config = $state.raw({ apiUrl: 'https://api.example.com' });

// ✅ Good: Snapshots for history
const snapshot = $state.snapshot(profile);
```

### Effects Patterns

```typescript
// ✅ Good: Effects with cleanup
$effect(() => {
  const timer = setInterval(() => {
    // Do something
  }, 1000);

  return () => clearInterval(timer);
});

// ✅ Good: Pre-effects for DOM measurements
$effect.pre(() => {
  const rect = element.getBoundingClientRect();
  // Use measurements before DOM updates
});
```

### Snippets Patterns

```svelte
<!-- ✅ Good: Parameterized snippets -->
{#snippet card(title, content, variant = 'default')}
  <div class="card card-{variant}">
    <h3>{title}</h3>
    <p>{content}</p>
  </div>
{/snippet}

<!-- ✅ Good: Passing snippets to components -->
<Card header={myHeader} content={myContent} />
```

## 🐛 Common Gotchas & Solutions

### TypeScript Integration

- Always use `<script lang="ts">` for TypeScript components
- Declare array types explicitly: `let items: any[] = $state([])`
- Import type definitions: `import type { Snippet } from 'svelte'`

### Event Handling

- Use modern syntax: `onclick` instead of `on:click`
- Don't mix old and new event syntaxes in the same component
- Use proper semantic elements for accessibility

### State Management

- Avoid updating state inside `$effect` (use `$derived` instead)
- Use `untrack()` to access state without creating dependencies
- Remember that `$state.raw` only triggers on reassignment

### Effects System

- Always clean up subscriptions and timers
- Use `$effect.pre` for DOM measurements
- Avoid infinite loops by being careful with dependencies

## 🌟 Key Learnings

This project demonstrates several important Svelte 5 concepts:

1. **Runes replace reactive statements**: The new `$state` and `$derived` runes provide more explicit and powerful reactivity
2. **Snippets replace slots**: More flexible component composition with typed parameters
3. **Enhanced TypeScript support**: Better type inference and error reporting  
4. **Modern event handling**: Cleaner syntax with better performance
5. **Advanced state management**: Fine-grained control over reactivity and side effects

## 🤝 Contributing

Contributions are welcome! This project serves as a learning resource for the Svelte community.

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/amazing-feature`
3. Commit your changes: `git commit -m 'Add amazing feature'`
4. Push to the branch: `git push origin feature/amazing-feature`
5. Open a Pull Request

## 📖 Resources

- [Svelte 5 Documentation](https://svelte.dev/docs/svelte/overview)
- [SvelteKit Documentation](https://svelte.dev/docs/kit/introduction)
- [Svelte 5 Migration Guide](https://svelte.dev/docs/svelte/v5-migration-guide)
- [Bun Documentation](https://bun.sh/docs)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- The Svelte team for creating an amazing framework
- The open-source community for continuous inspiration
- Contributors who help improve this showcase

---

**Built with ❤️ using Svelte 5, SvelteKit, Bun, and Tailwind CSS**

*This project is maintained as a learning resource for the Svelte community. Star ⭐ this repository if you find it helpful!*
