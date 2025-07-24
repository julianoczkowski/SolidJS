# Cursor Memories Guide for SolidJS + Modus Web Components

A comprehensive guide to using Cursor Memories for maintaining project context and improving AI assistance quality in SolidJS projects using Modus Web Components.

## What are Cursor Memories?

Cursor Memories are automatically generated rules based on your conversations in Chat that maintain context across sessions. According to the [Cursor documentation](https://docs.cursor.com/context/memories), memories are scoped to your project and help the AI remember important decisions, patterns, and preferences you've established.

## How Memories Work

### 1. Automatic Creation (Sidecar Observation)

- Cursor uses a background model that observes your conversations
- Automatically extracts relevant memories as you work
- **Requires user approval** before being saved for trust and control

### 2. Manual Creation (Tool Calls)

- Agent creates memories directly when you ask it to remember something
- Triggered when the AI notices important information for future sessions
- Can be created explicitly through chat commands

## Managing Memories

Access and manage memories through: **Cursor Settings → Rules**

## Essential Memories for SolidJS + Modus Web Components Projects

Here are essential memory templates to establish for any SolidJS project using Modus Web Components:

### 1. Framework & Architecture Memory

```markdown
**Memory: SolidJS Framework Standards**
This project uses SolidJS with TypeScript for reactive web development. Use signals (createSignal) for reactive state management, onMount for lifecycle management, and Show component for conditional rendering. Web components require client-side registration to prevent SSR hydration issues. Follow SolidJS reactive patterns and avoid traditional React patterns. File naming should be consistent (typically kebab-case for files, PascalCase for components).
```

### 2. Modus Web Components Memory

```markdown
**Memory: Modus Web Component Standards**
CRITICAL: Always use Modus Web Components from @trimble-oss/moduswebcomponents directly in JSX instead of native HTML elements. Use <modus-wc-button> instead of <button>, <modus-wc-text-input> instead of <input>, and <modus-wc-textarea> instead of <textarea>. Component categories include: Form Elements (modus-wc-button, modus-wc-text-input, modus-wc-select), Layout (modus-wc-card, modus-wc-divider, modus-wc-tabs), Feedback (modus-wc-alert, modus-wc-badge, modus-wc-progress), Overlays (modus-wc-modal, modus-wc-tooltip). All components require proper TypeScript declarations and mount guard patterns to prevent SSR issues.
```

### 3. Styling & Design System Memory

```markdown
**Memory: Modus Design System Standards**
All styling uses Modus CSS custom properties for theming (--modus-wc-color-primary, --modus-wc-color-base-page, etc.) to ensure consistent theming and automatic dark/light mode support. Modus Icons are the standard icon system using <i class="modus-icons">icon_name</i> pattern. Always validate icon names against available Modus icons before use. Dark mode support is automatic through CSS custom properties and theme switching via data-theme attribute.
```

### 4. Code Quality Memory

```markdown
**Memory: SolidJS Code Quality Standards**
Keep components focused and manageable (under 300 lines), functions concise (under 50 lines), and limit reactive state (maximum 5 createSignal calls per component). Use early returns, avoid deep nesting, and follow SolidJS reactive patterns. Include proper error handling, accessibility attributes (ARIA labels, keyboard navigation), and performance optimizations. Use createMemo for derived state and createEffect for side effects appropriately.
```

### 5. Component Development Memory

```markdown
**Memory: SolidJS Component Development Process**
When creating components, always import necessary SolidJS primitives (createSignal, onMount, Show, createMemo, etc.). Use Modus Web Components directly (<modus-wc-button>, <modus-wc-text-input>, etc.), implement proper TypeScript interfaces, and use CSS custom properties for styling. Components should be performant, accessible, and include proper error handling with loading states using Show component fallbacks.
```

### 6. Icon Usage Memory

```markdown
**Memory: Modus Icons Validation**
CRITICAL: Always validate Modus icon names before using them - many icons fail to render silently due to name mismatches. Common valid icons include: settings, person, delete, add, remove, search, close, check, warning, error, info, home, menu, arrow_up, arrow_down, arrow_left, arrow_right. When in doubt, verify icon names exist in the Modus icon set. Never import other icon libraries (Lucide, Heroicons, etc.) when using Modus Web Components.
```

### 7. Performance & Best Practices Memory

```markdown
**Memory: SolidJS Performance Standards**
Use SolidJS reactive patterns for optimal performance - signals update only when values change, createMemo for expensive derived computations, and createEffect for side effects with proper cleanup. Use dynamic imports for heavy components with loading states via Show component. Leverage SolidJS features (createSignal for state, onMount for initialization, createResource for async data). Implement proper error handling with Show component fallbacks and ErrorBoundary for component-level errors.
```

### 8. Development & Build Validation Memory

```markdown
**Memory: Development and Build Validation**
Always validate TypeScript compilation and component integration before completing tasks. Ensure all Modus Web Components are properly declared in TypeScript interfaces. Validate that mount guard patterns work correctly to prevent SSR hydration mismatches. Test component functionality in browser and verify that web component registration works properly for client-side rendering.
```

### 9. Color Management Memory

```markdown
**Memory: CSS Custom Properties for Colors**
CRITICAL: Always use Modus CSS custom properties for colors (--modus-wc-color-primary, --modus-wc-color-base-page, etc.) and never use hardcoded color values. All colors are defined by the Modus theme system and automatically switch between light/dark modes via data-theme attribute. Reference colors using var(--modus-wc-color-name) pattern. NEVER use hex colors (#ffffff), rgb colors, or hardcoded color values anywhere in components.
```

### 10. JSX Best Practices Memory

```markdown
**Memory: JSX Content Standards**
In JSX content, properly escape quotes to prevent linting errors. Use HTML entities for quotes: &ldquo; for opening quotes, &rdquo; for closing quotes, or &quot; for neutral quotes. Never use raw quotes (") in JSX text content as this causes linting violations. This applies to all JSX elements including text content in components. Always validate JSX syntax and fix quote-related issues immediately.
```

### 11. SSR and Hydration Memory

```markdown
**Memory: SSR and Web Component Hydration**
Modus Web Components require special handling for SSR. Use mount guard patterns with Show component to prevent hydration mismatches. Register web components client-side only and provide appropriate fallbacks during server rendering. Test SSR builds to ensure proper hydration and avoid common web component SSR issues.
```

### 12. TypeScript Integration Memory

```markdown
**Memory: TypeScript with Modus Web Components**
Properly declare all Modus Web Components in TypeScript interfaces to enable type checking and IntelliSense. Each new Modus component requires TypeScript declarations for its properties and events. Maintain strict TypeScript configuration and resolve all type errors. Use proper typing for component props, signals, and event handlers.
```

## How to Create New Memories

### Method 1: Explicit Request in Chat

Simply ask the AI to remember something specific:

```
"Please remember that we always use the 'primary' variant for call-to-action buttons in this project"
```

```
"Remember that we prefer composition over prop drilling for complex state management"
```

### Method 2: During Code Review/Discussion

When discussing patterns or making decisions, explicitly state:

```
"This is important - remember that we use createResource for all async data fetching in this project"
```

### Method 3: Correction-Based Learning

When correcting the AI's suggestions:

```
"Actually, in this project we always use createSignal with Modus Web Components for form handling instead of traditional form libraries. Please remember this for future form-related tasks."
```

### Method 4: Architecture Decisions

Document important architectural choices:

```
"Remember that we organize components by feature rather than by type in this project"
```

## Best Practices for Memory Creation

### 1. Be Specific and Contextual

- Include WHY a decision was made, not just WHAT
- Reference SolidJS-specific constraints or requirements
- Mention related technologies or patterns

### 2. Use Clear, Actionable Language

```markdown
✅ Good: "Always use createResource for data fetching in SolidJS components"
❌ Vague: "Data fetching is preferred"
```

### 3. Include Examples When Helpful

````markdown
"Remember that we use this specific error handling pattern:

```tsx
const [data, setData] = createSignal();
const [error, setError] = createSignal();

return (
  <Show when={!error()} fallback={<ErrorDisplay error={error()} />}>
    <Show when={data()} fallback={<div>Loading...</div>}>
      <DataDisplay data={data()} />
    </Show>
  </Show>
);
```
````

### 4. Update Memories When Patterns Evolve

- Review and update memories when architectural decisions change
- Remove outdated memories that no longer apply
- Merge related memories to avoid conflicts

## Memory Management Tips

### Regular Review

- Periodically review memories in **Cursor Settings → Rules**
- Remove or update obsolete memories
- Ensure memories don't conflict with each other

### Memory Hierarchy

1. **Framework Standards** - Core SolidJS and Modus Web Components patterns
2. **Project-Specific Patterns** - Your project's unique approaches
3. **Team Preferences** - Code style and development workflow preferences

### Testing Memory Effectiveness

- Ask the AI to explain project patterns to test memory retention
- Verify the AI suggests correct SolidJS approaches based on established memories
- Update memories if the AI consistently misses important context

## Common Memory Categories

### Technical Decisions

- SolidJS reactive patterns and state management approaches
- Modus Web Components integration strategies
- Performance optimization techniques

### Team Preferences

- Code style preferences beyond standard conventions
- Preferred debugging and development approaches
- Team-specific terminology or naming conventions

### Project-Specific Patterns

- Custom component architecture
- Specific business logic approaches
- Integration patterns with external services

### Lessons Learned

- Solutions to SolidJS-specific problems
- Modus Web Components integration challenges and solutions
- Performance bottlenecks and optimization strategies

## Example Memory Creation Sessions

### Session 1: Establishing Form Patterns

```
User: "In this project, we always use Modus Web Components with SolidJS signals for form handling"
AI: "I'll remember that this project uses Modus Web Components directly (<modus-wc-button>, <modus-wc-text-input>, <modus-wc-textarea>) with createSignal for reactive state management instead of native HTML elements or traditional form libraries."
```

### Session 2: Error Handling Preferences

```
User: "When API calls fail, we show error state using Show component with fallback"
AI: "Noted - I'll remember that error handling in this project uses SolidJS Show component with fallback patterns for API failures and async operations."
```

### Session 3: Component Organization

```
User: "We organize reusable components separately from page components, always using Modus Web Components"
AI: "I'll remember that this project separates reusable components from page components, always using Modus Web Components like <modus-wc-button>, <modus-wc-card>, <modus-wc-text-input> instead of native HTML elements."
```

### Session 4: State Management

```
User: "We use createSignal for local state and createStore for complex state objects"
AI: "I'll remember that state management in this project uses createSignal for simple reactive values and createStore for complex state objects that need fine-grained reactivity."
```

### Session 5: Styling Approach

```
User: "When styling, always use Modus CSS custom properties - never use hardcoded colors"
AI: "I'll remember that all styling must use Modus CSS custom properties (--modus-wc-color-primary, --modus-wc-color-base-page, etc.) for theming. Never use inline colors, hex codes, or hardcoded color values to ensure automatic dark/light mode support through the Modus design system."
```

### Session 6: Icon Usage

```
User: "Before using any Modus icon, verify the icon name exists in the Modus icon set"
AI: "I'll remember that before using ANY Modus icon, I must verify the icon name exists in the official Modus icon set. Never guess icon names or assume standard names exist. Invalid icon names fail silently, breaking the UI. Common valid icons include: settings, person, delete, add, remove, search, close, check."
```

By establishing these foundational memories and following the creation guidelines, you'll have a more intelligent and context-aware AI assistant that understands SolidJS reactive patterns and Modus Web Components integration. The memories will help maintain consistency in your development approach and catch common issues before they become problems.

## Benefits of Using This Memory System

- **Consistency**: Ensures all team members and AI assistance follow the same patterns
- **Quality**: Prevents common mistakes with SolidJS reactivity and Modus Web Components
- **Efficiency**: Reduces time spent explaining the same concepts repeatedly
- **Learning**: Captures institutional knowledge about best practices and lessons learned
- **Integration**: Maintains proper TypeScript integration and SSR compatibility
