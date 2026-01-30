# Frontend Component Generation Guide for AI Models (2025 Edition)

## Purpose
This guide defines the standards, constraints, and best practices that AI must follow when generating frontend components. All generated code must prioritize **readability, scalability, accessibility, performance, and real-world production readiness**.

The AI should treat this guide as **mandatory** unless explicitly overridden by the user.

---

## 1. General Principles

### 1.1 Context-First Development (The "Occasion" Mandate)
Before writing any code, analyze the user's prompt for the specific **occasion** or **context**. AI must adopt appropriate personas based on use case:

**Enterprise/SaaS Persona:**
- Rigid structure, high data density
- Subtle interactions, perfect alignment
- Professional color schemes, conservative typography
- Focus on efficiency and data clarity

**Consumer/Marketing Persona:**
- Large typography, generous whitespace
- Emotive imagery, engaging motion
- Brand-focused design, emotional connection
- Conversion-optimized interactions

**Internal Tools Persona:**
- High efficiency, keyboard shortcuts
- Dense information display
- Utility-focused, minimal decoration
- Power user features and shortcuts

**Example Context Analysis:**
- ❌ **Generic**: "Make a button"
- ✅ **Context-Aware**: "Create a primary CTA for a fintech dashboard where users initiate high-value transfers"

### 1.2 Zero-Generic Policy (Anti-Template Rules)
**Strictly Forbidden Patterns:**
- ❌ **No "Lorem Ipsum"** - Use realistic, industry-specific copy
- ❌ **No "Boxy" layouts** - Use varied border radii, asymmetrical layouts
- ❌ **No flat colors** - Use gradients, subtle noise, multi-layered shadows
- ❌ **No default shadows** - Stack shadows for realistic elevation
- ❌ **No generic placeholders** - "Title Here" → "Q4 Revenue Dashboard"

**Mandatory Enhancements:**
- ✅ **Purpose-driven spacing** with contextual comments
- ✅ **Realistic content** that matches the use case
- ✅ **Depth and texture** through layered visual effects
- ✅ **Brand-appropriate typography** and color choices

### 1.3 Core Philosophy
- **Clarity over cleverness** — readable code beats "smart" code
- **Production-quality, not demo code** — assume this will be maintained by a team
- **Avoid over-engineering** — solve the problem at hand, not hypothetical future problems
- **Explicit over implicit** — make intentions clear in code
- **Component-driven architecture** — build reusable, isolated, composable components
- **Performance by default** — optimize for Core Web Vitals and user experience
- **Context is king** — solve specific human moments, not generic problems

### Modern Frontend Mindset (2025)
- Assume **TypeScript by default** unless told otherwise
- Prefer **server-side rendering (SSR) / static generation (SSG)** patterns when relevant
- Think in terms of **design systems** and **design tokens**, not one-off styles
- Build for **multiple devices and screen sizes** from the start
- Consider **edge computing** and distributed rendering patterns

---

## 2. Component Design Rules

### 2.1 Component Scope & Responsibility
- Each component should have **one clear, well-defined responsibility**
- If logic or UI becomes complex (>150 lines), **suggest splitting into subcomponents**
- **Separate concerns**: presentation vs. logic vs. data fetching
- Do NOT bundle unrelated functionality into one component

**Example of good separation:**
```tsx
// ❌ BAD: Everything in one component
function UserDashboard() {
  // 300 lines of mixed concerns
}

// ✅ GOOD: Clear separation
function UserDashboard() {
  return (
    <>
      <DashboardHeader />
      <UserStats />
      <ActivityFeed />
    </>
  )
}
```

### 2.2 Naming Conventions
Follow established naming patterns rigorously:

- **Components**: `PascalCase` (e.g., `UserProfileCard`, `NavigationMenu`)
- **Hooks**: `useSomething` (e.g., `useAuth`, `useLocalStorage`)
- **Props & Variables**: `camelCase` (e.g., `isLoading`, `userData`)
- **Event Handlers**: `handleAction` or `onAction` (e.g., `handleSubmit`, `onClick`)
- **Constants**: `UPPER_SNAKE_CASE` (e.g., `MAX_RETRY_ATTEMPTS`)
- **Files**: Match component name (e.g., `UserProfileCard.tsx`)

**Bad:**
```tsx
function Comp1({ data2, fn }) { }
```

**Good:**
```tsx
function UserProfileCard({ userData, onUpdate }) { }
```

### 2.4 Compound Components Pattern (The Golden Rule)
Avoid "God Components" with 30+ props. Use **Compound Components** or **Slots** for flexibility:

**❌ Bad (Rigid):**
```tsx
<ProfileCard 
  name="Alex" 
  bio="Building interface agnostic systems..." 
  showButton={true} 
  buttonText="Follow" 
  avatarSrc="..."
/>
```

**✅ Good (Flexible):**
```tsx
<Card variant="elevated">
  <CardHeader>
    <Avatar src="alex-avatar.jpg" status="online" />
    <div className="flex flex-col">
       <Text variant="h3">Alex Doe</Text>
       <Text variant="muted">Product Designer</Text>
    </div>
  </CardHeader>
  <CardContent>
     <p>Building interface agnostic systems that scale across platforms...</p>
  </CardContent>
  <CardFooter>
     <Button icon={<PlusIcon />}>Follow</Button>
  </CardFooter>
</Card>
```

**Implementation Pattern:**
```tsx
function Card({ children, variant = 'default' }: CardProps) {
  return (
    <div className={`card card--${variant}`}>
      {children}
    </div>
  )
}

Card.Header = function CardHeader({ children }: { children: React.ReactNode }) {
  return <div className="card-header">{children}</div>
}

Card.Content = function CardContent({ children }: { children: React.ReactNode }) {
  return <div className="card-content">{children}</div>
}

Card.Footer = function CardFooter({ children }: { children: React.ReactNode }) {
  return <div className="card-footer">{children}</div>
}
```
### 2.5 File Organization
Use **feature-based folder structure** for scalability:

```
src/
├── components/
│   ├── ui/           # Shared UI primitives (Button, Input, Card)
│   └── features/     # Feature-specific components
├── hooks/            # Custom hooks
├── utils/            # Helper functions
├── styles/           # Global styles, design tokens
├── types/            # TypeScript type definitions
└── config/           # Configuration files
```

---

## 3. Props & API Design

### 3.1 Principles
- Design props as if **the component will be reused across the entire app**
- Prefer **explicit, typed props** over large config objects
- Always define required vs. optional props
- Provide **sensible default values** for optional props
- Follow the **Open-Closed Principle**: open for extension, closed for modification

### 3.2 TypeScript Interface Design
```tsx
// ✅ GOOD: Clear, explicit, well-documented
interface ButtonProps {
  /** Button label text */
  label: string
  /** Visual style variant */
  variant?: 'primary' | 'secondary' | 'ghost'
  /** Size of the button */
  size?: 'sm' | 'md' | 'lg'
  /** Disabled state */
  disabled?: boolean
  /** Loading state with spinner */
  isLoading?: boolean
  /** Click handler */
  onClick: () => void
  /** Optional icon to display */
  icon?: React.ReactNode
  /** Additional CSS classes */
  className?: string
}

function Button({ 
  label, 
  variant = 'primary',
  size = 'md',
  disabled = false,
  isLoading = false,
  onClick,
  icon,
  className 
}: ButtonProps) {
  // Implementation
}
```

### 3.3 Composition Patterns
Favor **composition over configuration** when possible:

```tsx
// ✅ GOOD: Compositional API
<Card>
  <CardHeader>
    <CardTitle>User Profile</CardTitle>
  </CardHeader>
  <CardContent>
    {/* content */}
  </CardContent>
</Card>

// ❌ AVOID: Heavy configuration
<Card 
  title="User Profile"
  headerConfig={{ /* ... */ }}
  contentConfig={{ /* ... */ }}
/>
```

---

## 4. State Management

### 4.1 State Location Strategy
- **Local state first** — use `useState` when state is only needed in one component
- **Lift state up** only when multiple components need shared state
- **Context for theme/auth** — use React Context for truly global, infrequently-changing state
- **External state managers** (Zustand, Redux Toolkit, Tanstack Query) for complex client state or server data

### 4.2 State Minimization
Keep state **minimal and derived**:

```tsx
// ❌ BAD: Duplicate state
const [users, setUsers] = useState([])
const [activeUsers, setActiveUsers] = useState([])

// ✅ GOOD: Single source of truth, derive the rest
const [users, setUsers] = useState([])
const activeUsers = users.filter(user => user.isActive)
```

### 4.3 Server State vs Client State
- **Server state** (data from APIs): Use `Tanstack Query`, `SWR`, or similar
- **Client state** (UI state): Use `useState`, `useReducer`, or lightweight state managers
- **Do not mix them** — keep server data separate from UI state

---

## 5. Styling & Design System Integration

### 5.1 General Styling Rules
- **Consistency is paramount** — follow the project's design system
- Avoid inline styles unless absolutely necessary (dynamic values)
- Use established styling methodology:
  - **CSS Modules** for component-scoped styles
  - **Tailwind CSS** for utility-first approach
  - **Styled Components / Emotion** for CSS-in-JS
  - Use whichever is specified in the prompt

### 5.2 Design Tokens (Critical for 2025)
Design tokens are the **single source of truth** for design decisions. Always use tokens instead of hardcoded values.

**Bad:**
```tsx
<div style={{ color: '#3B82F6', padding: '16px', borderRadius: '8px' }}>
```

**Good (Tailwind):**
```tsx
<div className="text-primary-500 p-4 rounded-lg">
```

**Good (CSS Variables):**
```css
.card {
  color: var(--color-primary);
  padding: var(--spacing-4);
  border-radius: var(--radius-lg);
}
```

### 5.3 Design Token Categories
When implementing design systems, organize tokens into:

1. **Foundation Tokens** (primitives): Raw values
   - Colors: `--color-blue-500: #3B82F6`
   - Spacing: `--spacing-4: 1rem`
   - Typography: `--font-sans: 'Inter', sans-serif`

2. **Semantic Tokens**: Contextual meaning
   - `--color-primary: var(--color-blue-500)`
   - `--spacing-section: var(--spacing-8)`

3. **Component Tokens**: Component-specific
   - `--button-padding: var(--spacing-3) var(--spacing-6)`
   - `--card-border-radius: var(--radius-lg)`

### 5.4 Responsive Design
Use **mobile-first** approach with design tokens:

```tsx
// ✅ Tailwind mobile-first
<div className="w-full md:w-1/2 lg:w-1/3">

// ✅ CSS mobile-first
.container {
  width: 100%;
  
  @media (min-width: 768px) {
    width: 50%;
  }
  
  @media (min-width: 1024px) {
    width: 33.333%;
  }
}
```

---

## 6. Accessibility (WCAG 2.1 Level AA Minimum)

### 6.1 Non-Negotiable Requirements
Every interactive component **must**:

- ✅ Use **semantic HTML elements** (`<button>`, `<nav>`, `<main>`, `<article>`, etc.)
- ✅ Support **keyboard navigation** (Tab, Enter, Space, Arrow keys where appropriate)
- ✅ Include proper **ARIA attributes** when semantic HTML is insufficient
- ✅ Associate **labels with inputs** (`<label htmlFor="...">` or `aria-label`)
- ✅ Handle **focus states** visibly (`:focus-visible`)
- ✅ Maintain **color contrast ratios** (4.5:1 for normal text, 3:1 for large text)
- ✅ Provide **alternative text** for images and icons
- ✅ Support **screen readers** with proper ARIA labels and descriptions

### 6.2 Common Patterns

**Buttons:**
```tsx
// ✅ Accessible button
<button
  onClick={handleClick}
  disabled={isLoading}
  aria-label="Delete user account"
  aria-busy={isLoading}
>
  {isLoading ? <Spinner aria-hidden="true" /> : 'Delete Account'}
</button>
```

**Form Inputs:**
```tsx
// ✅ Accessible input
<div>
  <label htmlFor="email" className="block mb-2">
    Email Address
  </label>
  <input
    id="email"
    type="email"
    required
    aria-required="true"
    aria-invalid={!!emailError}
    aria-describedby={emailError ? "email-error" : undefined}
  />
  {emailError && (
    <p id="email-error" role="alert" className="text-red-600">
      {emailError}
    </p>
  )}
</div>
```

**Modals/Dialogs:**
```tsx
// ✅ Accessible modal
<dialog
  role="dialog"
  aria-modal="true"
  aria-labelledby="modal-title"
  aria-describedby="modal-description"
>
  <h2 id="modal-title">Confirm Action</h2>
  <p id="modal-description">Are you sure you want to proceed?</p>
  {/* content */}
</dialog>
```

### 6.3 Motion & Animation Accessibility
Respect user preferences for reduced motion:

```css
.animated-element {
  transition: transform 0.3s ease;
}

@media (prefers-reduced-motion: reduce) {
  .animated-element {
    transition: none;
  }
}
```

---

## 7. Micro-interactions & Animation

### 7.1 Purpose of Micro-interactions
Micro-interactions enhance UX by:
- Providing **immediate feedback** to user actions
- **Guiding users** through interfaces
- Creating **emotional connections** with delightful moments
- **Reducing cognitive load** by showing system status

### 7.2 Core Principles
1. **Purposeful**: Every animation should have a clear reason
2. **Subtle**: Not distracting or overwhelming (300-500ms is ideal)
3. **Performant**: Use CSS transforms and opacity for 60fps animations
4. **Accessible**: Respect `prefers-reduced-motion`

### 7.3 Premium Feel Micro-interactions (The "Soul" Mandate)
Static interfaces feel cheap. Every component must inject **soul** through thoughtful interactions:

**Cursor States (Non-Negotiable):**
```tsx
// Every interactive element needs proper cursor feedback
<button className="cursor-pointer">Click me</button>
<input className="cursor-text" readOnly />
<div className="cursor-not-allowed opacity-50">Disabled action</div>
```

**Active Feedback (Mandatory):**
```tsx
// Buttons must visually respond to interaction
<button className="
  transform active:scale-95           // Physical press feedback
  transition-transform duration-100   // Smooth but quick
  hover:bg-primary-dark               // Hover state
  focus:ring-2 focus:ring-offset-2   // Keyboard focus
">
  Submit
</button>
```

**Transition Wrappers (Entry Animations):**
```tsx
// Elements entering DOM must animate smoothly
function AnimatedList({ items }) {
  return (
    <motion.div
      initial={{ opacity: 0, y: 20 }}
      animate={{ opacity: 1, y: 0 }}
      transition={{ duration: 0.3 }}
    >
      {items.map(item => (
        <motion.div
          key={item.id}
          initial={{ opacity: 0 }}
          animate={{ opacity: 1 }}
          transition={{ delay: 0.1 }}
        >
          {item.content}
        </motion.div>
      ))}
    </motion.div>
  )
}
```

**Focus Management (Accessibility First):**
```tsx
// Custom focus rings that match your design system
function FocusableInput() {
  return (
    <input className="
      focus:outline-none                    // Remove default
      focus:ring-2                         // Custom ring
      focus:ring-blue-500                   // Brand color
      focus:ring-offset-2                  // Spacing from element
      focus:ring-offset-white              // Match background
    " />
  )
}
```

**Loading States with Personality:**
```tsx
// Beyond basic spinners - match component geometry
function SmartButton({ isLoading, children, ...props }) {
  return (
    <button 
      {...props}
      disabled={isLoading}
      className="relative overflow-hidden"
    >
      {/* Content transition */}
      <span className={cn(
        "transition-all duration-300",
        isLoading ? "opacity-0 transform -translate-y-2" : "opacity-100"
      )}>
        {children}
      </span>
      
      {/* Loading state that matches button */}
      {isLoading && (
        <div className="absolute inset-0 flex items-center justify-center">
          <div className="w-4 h-4 border-2 border-white border-t-transparent rounded-full animate-spin" />
        </div>
      )}
    </button>
  )
}
```

### 7.4 Common Micro-interaction Patterns

**Button Hover/Click:**
```tsx
// Tailwind with hover states
<button className="
  bg-primary hover:bg-primary-dark 
  transform hover:scale-105 active:scale-95
  transition-all duration-200 ease-out
">
  Click Me
</button>
```

**Loading States:**
```tsx
function SubmitButton({ isLoading, onSubmit }) {
  return (
    <button 
      onClick={onSubmit} 
      disabled={isLoading}
      className="relative"
    >
      <span className={isLoading ? 'opacity-0' : 'opacity-100'}>
        Submit
      </span>
      {isLoading && (
        <div className="absolute inset-0 flex items-center justify-center">
          <Spinner />
        </div>
      )}
    </button>
  )
}
```

**Success Feedback:**
```tsx
function LikeButton() {
  const [liked, setLiked] = useState(false)
  
  return (
    <button
      onClick={() => setLiked(!liked)}
      className={`
        transition-transform duration-200
        ${liked ? 'scale-110 text-red-500' : 'scale-100 text-gray-400'}
      `}
      aria-label={liked ? 'Unlike' : 'Like'}
      aria-pressed={liked}
    >
      <HeartIcon className={liked ? 'fill-current' : ''} />
    </button>
  )
}
```

**Skeleton Screens (better than spinners for content loading):**
```tsx
function UserCardSkeleton() {
  return (
    <div className="animate-pulse space-y-4">
      <div className="h-12 bg-gray-200 rounded-full w-12" />
      <div className="h-4 bg-gray-200 rounded w-3/4" />
      <div className="h-4 bg-gray-200 rounded w-1/2" />
    </div>
  )
}
```

### 7.4 Animation Performance
- ✅ **Use CSS transforms** (`translate`, `scale`, `rotate`) and `opacity` — GPU accelerated
- ❌ **Avoid animating** `width`, `height`, `top`, `left` — causes layout thrashing
- Use `will-change` sparingly and only when necessary

---

## 8. Defensive Engineering & Production Readiness

### 8.1 The Four States Pattern + Graceful Failures
Every component must handle edge cases realistically:

**1. Loading State** — Never leave users guessing
**2. Success State** — Display the data beautifully  
**3. Error State** — Friendly messages with recovery options
**4. Empty State** — Guide users when no data exists
**5. Graceful Failures** — What happens when things break?

```tsx
function UserProfile({ userId }: { userId: string }) {
  const { data: user, isLoading, error } = useUser(userId)
  
  if (isLoading) return <UserProfileSkeleton />
  if (error) return <ErrorMessage error={error} onRetry={refetch} />
  if (!user) return <EmptyState message="User not found" />
  
  return (
    <Card>
      {/* Image failure handling */}
      <Avatar 
        src={user.avatar} 
        fallback={<UserIcon className="w-12 h-12" />}
        onError={(e) => {
          // Fallback when image fails to load
          e.currentTarget.style.display = 'none'
          e.currentTarget.nextElementSibling?.classList.remove('hidden')
        }}
      />
      {/* Text overflow handling */}
      <Text variant="h3" className="truncate" title={user.name}>
        {user.name}
      </Text>
    </Card>
  )
}
```

### 8.2 Text Overflow & Content Constraints
Always handle realistic content scenarios:

```tsx
// ❌ BAD: Assumes perfect content
function UserCard({ name, bio }: { name: string; bio: string }) {
  return (
    <div>
      <h3>{name}</h3> {/* What if name is 500 chars? */}
      <p>{bio}</p>    {/* What if bio is empty or massive? */}
    </div>
  )
}

// ✅ GOOD: Handles real-world content
function UserCard({ name, bio }: { name: string; bio: string }) {
  return (
    <div>
      <h3 
        className="font-semibold truncate" 
        title={name.length > 50 ? name : undefined}
      >
        {name}
      </h3>
      <p className="text-sm text-gray-600 line-clamp-3">
        {bio || <span className="italic text-gray-400">No bio available</span>}
      </p>
    </div>
  )
}
```

### 8.3 Image & Asset Failure Handling
Never assume assets load successfully:

```tsx
function RobustImage({ 
  src, 
  alt, 
  fallback, 
  className 
}: ImageProps) {
  const [hasError, setHasError] = useState(false)
  const [isLoading, setIsLoading] = useState(true)
  
  if (hasError && fallback) {
    return <div className={className}>{fallback}</div>
  }
  
  return (
    <div className="relative">
      {isLoading && (
        <div className="absolute inset-0 bg-gray-200 animate-pulse" />
      )}
      <img
        src={src}
        alt={alt}
        className={cn(
          "transition-opacity duration-300",
          isLoading ? "opacity-0" : "opacity-100",
          className
        )}
        onLoad={() => setIsLoading(false)}
        onError={() => {
          setHasError(true)
          setIsLoading(false)
        }}
      />
    </div>
  )
}
```
Every component that deals with async data should handle:

1. **Loading state** — show progress indication
2. **Success state** — display the data
3. **Error state** — show friendly error messages with recovery options
4. **Empty state** — guide users when no data exists

```tsx
function UserList() {
  const { data, isLoading, error } = useUsers()
  
  if (isLoading) return <UserListSkeleton />
  if (error) return <ErrorMessage error={error} onRetry={refetch} />
  if (!data?.length) return <EmptyState message="No users found" />
  
  return <div>{/* render users */}</div>
}
```

### 9.2 Error Messages
- Be **specific and actionable**
- Avoid technical jargon in user-facing errors
- Provide **recovery options** when possible
- Log technical details to console/monitoring tools

```tsx
// ❌ BAD
<p>Error: Failed to fetch</p>

// ✅ GOOD
<div className="bg-red-50 border border-red-200 rounded p-4">
  <h3 className="font-semibold text-red-900">Unable to Load Users</h3>
  <p className="text-red-700">
    We couldn't connect to the server. Please check your internet connection.
  </p>
  <button onClick={retry} className="mt-2 text-red-600 underline">
    Try Again
  </button>
</div>
```

### 9.3 Form Validation
- **Validate on blur** for better UX (not on every keystroke)
- Show **inline validation** messages near the field
- Disable submit button until form is valid
- Provide clear **error summaries** at the top for complex forms

---

## 10. Performance Optimization

### 10.1 Core Web Vitals Focus
Optimize for the metrics that matter:
- **LCP (Largest Contentful Paint)**: < 2.5s
- **FID (First Input Delay)**: < 100ms  
- **CLS (Cumulative Layout Shift)**: < 0.1

### 10.2 Performance Best Practices

**Code Splitting:**
```tsx
// Lazy load heavy components
const HeavyChart = lazy(() => import('./HeavyChart'))

function Dashboard() {
  return (
    <Suspense fallback={<ChartSkeleton />}>
      <HeavyChart />
    </Suspense>
  )
}
```

**Image Optimization:**
```tsx
// Use Next.js Image or similar
<Image
  src="/hero.jpg"
  alt="Hero image"
  width={1200}
  height={600}
  priority // for above-fold images
  loading="lazy" // for below-fold images
  placeholder="blur" // show blur while loading
/>
```

**Memoization (use judiciously):**
```tsx
// Expensive calculations
const expensiveValue = useMemo(
  () => computeExpensiveValue(data),
  [data]
)

// Prevent unnecessary re-renders
const MemoizedComponent = memo(Component)

// Stable callback references
const handleClick = useCallback(() => {
  doSomething(id)
}, [id])
```

**Virtual Scrolling for Long Lists:**
```tsx
// Use libraries like react-window or react-virtual
import { FixedSizeList } from 'react-window'

<FixedSizeList
  height={600}
  itemCount={1000}
  itemSize={50}
  width="100%"
>
  {Row}
</FixedSizeList>
```

---

## 10. Data Fetching & Side Effects

### 10.1 Separation of Concerns
- **Keep data fetching logic separate** from presentation components
- Use **custom hooks** to encapsulate fetching logic
- Prefer **server-side data fetching** (SSR/SSG) when possible

### 10.2 Modern Data Fetching Patterns

**With Tanstack Query (recommended for client-side):**
```tsx
function useUsers() {
  return useQuery({
    queryKey: ['users'],
    queryFn: fetchUsers,
    staleTime: 5 * 60 * 1000, // 5 minutes
    retry: 3,
  })
}

function UserList() {
  const { data, isLoading, error } = useUsers()
  // Component focuses on presentation
}
```

**With Server Components (Next.js App Router):**
```tsx
// app/users/page.tsx - Server Component
async function UsersPage() {
  const users = await fetchUsers() // Fetched on server
  return <UserList users={users} />
}

// UserList.tsx - Client Component for interactivity
'use client'
function UserList({ users }) {
  // Just presentation + client-side interactions
}
```

### 10.3 useEffect Guidelines
- **Avoid side effects in render** — always use `useEffect`
- **Cleanup subscriptions** and timers
- **Specify dependencies accurately** — no empty arrays unless intentional
- Consider if you even need useEffect (often you don't)

```tsx
// ✅ GOOD: Proper cleanup
useEffect(() => {
  const subscription = api.subscribe(id)
  
  return () => {
    subscription.unsubscribe()
  }
}, [id])

// ❌ BAD: Side effect in render
function Component() {
  localStorage.setItem('key', value) // DON'T DO THIS
  return <div />
}
```

---

## 11. Code Quality & Structure

### 11.1 Component Organization
Structure components for readability:

```tsx
// 1. Imports (group by type)
import { useState, useEffect } from 'react'
import { useQuery } from '@tanstack/react-query'
import { Button, Card } from '@/components/ui'
import { formatDate } from '@/utils'
import type { User } from '@/types'

// 2. Type definitions
interface UserCardProps {
  user: User
  onEdit: (id: string) => void
}

// 3. Component
export function UserCard({ user, onEdit }: UserCardProps) {
  // 3a. Hooks (state, queries, effects)
  const [isExpanded, setIsExpanded] = useState(false)
  const { data } = useUserDetails(user.id)
  
  // 3b. Derived values
  const displayName = `${user.firstName} ${user.lastName}`
  
  // 3c. Event handlers
  const handleEdit = () => onEdit(user.id)
  const handleToggle = () => setIsExpanded(!isExpanded)
  
  // 3d. Early returns
  if (!user) return null
  
  // 3e. Main render
  return (
    <Card>
      {/* JSX */}
    </Card>
  )
}

// 4. Sub-components (if small and only used here)
function UserStats({ stats }: { stats: Stats }) {
  return <div>{/* ... */}</div>
}
```

### 11.2 JSX Best Practices
- **Avoid deeply nested JSX** (>3 levels) — extract to sub-components
- Use **early returns** for conditional rendering
- **Extract complex logic** into helper functions or custom hooks
- Use **fragment shorthand** `<>` when appropriate

```tsx
// ❌ BAD: Deeply nested, hard to read
return (
  <div>
    {user ? (
      <div>
        {user.isActive ? (
          <div>
            {user.posts.length > 0 ? (
              <div>{/* ... */}</div>
            ) : null}
          </div>
        ) : null}
      </div>
    ) : null}
  </div>
)

// ✅ GOOD: Early returns, extracted components
if (!user) return null
if (!user.isActive) return <InactiveUserMessage />
if (!user.posts.length) return <EmptyPostsState />

return <UserPostsList posts={user.posts} />
```

### 11.3 Professional Commenting Standards

**Comment Like a Senior Engineer, Not an AI:**

- **Comment WHY, not WHAT** — code should be self-explanatory
- **Comment business logic that isn't obvious** from the code itself
- **Comment performance-critical decisions** and trade-offs
- **Comment complex algorithms** or non-standard patterns
- **NEVER use icons in comments** — no one types `// 🚨` or `// 💡` in real code
- **Avoid obvious comments** that just restate what the code does

**Professional Examples:**
```tsx
// ❌ BAD: Obvious, AI-like comments
// Set loading to true
setIsLoading(true)

// ✅ GOOD: Explains why
// Debounce search to avoid excessive API calls
const debouncedSearch = useDebouncedValue(searchQuery, 300)

/**
 * Calculates user's subscription tier based on usage metrics.
 * Premium tier requires >1000 API calls/month AND >5 team members.
 */
function calculateSubscriptionTier(usage: Usage): Tier {
  // Implementation
}
```

---

## 12. Testing Considerations

### 12.1 Testability Guidelines
When generating components, structure them to be easily testable:

- **Avoid tight coupling** to external dependencies
- **Accept dependencies via props** (dependency injection)
- **Separate logic from presentation** (custom hooks)
- **Use test IDs** for reliable selection in tests

```tsx
// ✅ Testable: Dependencies injected
function UserList({ 
  users, 
  onUserClick,
  EmptyComponent = DefaultEmpty 
}: UserListProps) {
  // Easy to test with mock data
}

// ❌ Hard to test: Direct API calls, tight coupling
function UserList() {
  const [users, setUsers] = useState([])
  
  useEffect(() => {
    fetch('/api/users').then(/* ... */)
  }, [])
}
### 12.2 Test IDs
Add `data-testid` attributes for complex components:

```tsx
<button 
  data-testid="submit-button"
  onClick={handleSubmit}
>
  Submit
</button>
- ❌ No unvalidated user input reaching backend

### 14.3 Handling Ambiguity

If requirements are unclear or ambiguous:

1. **Make a reasonable assumption** based on modern best practices
2. **Clearly state the assumption** in your response
3. **Offer alternatives** if appropriate
4. **Ask for clarification** if the decision significantly impacts the implementation

**Example:**
> "I'm assuming you want server-side data fetching since this is a Next.js app. If you need client-side fetching instead, I can refactor to use Tanstack Query."

---

## 15. Advanced Patterns & Considerations

### 15.1 Compound Components
For complex, configurable components:

```tsx
// Flexible, composable API
function Select({ children, value, onChange }) {
  return (
    <SelectContext.Provider value={{ value, onChange }}>
      <div className="select-wrapper">
        {children}
      </div>
    </SelectContext.Provider>
  )
}

Select.Trigger = function SelectTrigger({ children }) { /* ... */ }
Select.Content = function SelectContent({ children }) { /* ... */ }
Select.Item = function SelectItem({ children, value }) { /* ... */ }

// Usage
<Select value={selected} onChange={setSelected}>
  <Select.Trigger>Choose...</Select.Trigger>
  <Select.Content>
    <Select.Item value="1">Option 1</Select.Item>
    <Select.Item value="2">Option 2</Select.Item>
  </Select.Content>
</Select>
```

### 15.2 Render Props & Children as Function
When components need to share logic but not UI:

```tsx
function DataFetcher({ url, children }) {
  const { data, isLoading, error } = useQuery({ queryKey: [url], queryFn: () => fetch(url) })
  return children({ data, isLoading, error })
}

// Usage
<DataFetcher url="/api/users">
  {({ data, isLoading, error }) => {
    if (isLoading) return <Spinner />
    if (error) return <Error />
    return <UserList users={data} />
  }}
</DataFetcher>
```

### 15.3 Internationalization (i18n)
For apps supporting multiple languages:

- **Don't hardcode strings** — use translation keys
- Support **RTL (right-to-left) languages** when applicable
- Use libraries like `react-i18next` or `next-intl`

```tsx
import { useTranslation } from 'react-i18next'

function WelcomeMessage() {
  const { t } = useTranslation()
  
  return (
    <h1>{t('welcome.title', { name: user.name })}</h1>
  )
}
```

### 15.4 Theme Support
Design for theme switching from the start:

```tsx
// Using CSS variables for theming
// themes.css
[data-theme="light"] {
  --bg-primary: #ffffff;
  --text-primary: #000000;
}

[data-theme="dark"] {
  --bg-primary: #1a1a1a;
  --text-primary: #ffffff;
}

// Component
function ThemedCard() {
  return (
    <div style={{ 
      backgroundColor: 'var(--bg-primary)',
      color: 'var(--text-primary)'
    }}>
      {/* content */}
    </div>
  )
}
```

---

## 16. Real-World Production Checklist

Before considering a component "production-ready," verify:

### Functionality
- [ ] Component works as intended with real data
- [ ] All edge cases handled (empty, error, loading states)
- [ ] No console errors or warnings
- [ ] Works across target browsers

### Accessibility
- [ ] Semantic HTML used correctly
- [ ] Keyboard navigation works
- [ ] Screen reader tested (at least basic NVDA/VoiceOver test)
- [ ] Color contrast meets WCAG 2.1 AA
- [ ] Focus states visible and logical
- [ ] ARIA labels present where needed

### Performance
- [ ] No unnecessary re-renders
- [ ] Large lists virtualized
- [ ] Images optimized and lazy-loaded
- [ ] Code splitting applied for large components
- [ ] No layout shifts (CLS < 0.1)

### Maintainability  
- [ ] TypeScript types defined
- [ ] Naming conventions followed
- [ ] Component properly documented
- [ ] Testable structure
- [ ] No hardcoded values (use design tokens)

### Security
- [ ] User input sanitized/validated
- [ ] No XSS vulnerabilities
- [ ] No exposed API keys or secrets
- [ ] HTTPS enforced for API calls

---

## 17. Common Mistakes to Avoid

### ❌ Anti-patterns
1. **Prop drilling hell** — use Context or state management
2. **useState abuse** — use useReducer for complex state
3. **Inline arrow functions in JSX** — causes unnecessary re-renders
4. **Missing key props in lists** — use stable, unique keys
5. **Mutating state directly** — always create new objects/arrays
6. **Forgetting cleanup in useEffect** — leads to memory leaks
7. **Over-abstraction** — don't create abstractions for things used once
8. **Premature optimization** — measure first, optimize later

### ✅ Best Practices Summary
1. **Component composition** over configuration
2. **Declarative over imperative** code
3. **Type safety** everywhere (TypeScript)
4. **Accessibility** from the start, not as an afterthought
5. **Performance by default** (lazy loading, memoization where needed)
6. **Consistent styling** via design systems and tokens
7. **Test-driven mindset** (structure for testability)
8. **User-centric** — always prioritize user experience

---

## 18. Progressive Enhancement & Graceful Degradation

### 18.1 Core Philosophy
Build components that work everywhere, enhance where possible:

- **Base functionality**: Works without JavaScript, CSS, or modern APIs
- **Enhanced experience**: Adds interactivity, animations, advanced features
- **Graceful fallbacks**: Degrade gracefully when features unavailable

### 18.2 Progressive Enhancement Layers

**Layer 1: Semantic HTML (Always Works)**
```html
<!-- Base functionality without JS -->
<form action="/submit" method="POST">
  <label for="email">Email</label>
  <input id="email" name="email" type="email" required>
  <button type="submit">Subscribe</button>
</form>
```

**Layer 2: CSS Enhancement**
```css
/* Enhance with styling and transitions */
button {
  transition: transform 0.2s ease;
}

button:hover {
  transform: translateY(-2px);
}

/* Reduced motion support */
@media (prefers-reduced-motion: reduce) {
  button { transition: none; }
}
```

**Layer 3: JavaScript Enhancement**
```tsx
// Enhance with client-side validation and feedback
function SubscriptionForm() {
  const [isSubmitting, setIsSubmitting] = useState(false)
  
  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault() // Prevent default only if JS available
    setIsSubmitting(true)
    
    try {
      await submitForm(formData)
      showSuccessMessage()
    } catch (error) {
      showErrorMessage(error)
    } finally {
      setIsSubmitting(false)
    }
  }

  return (
    <form onSubmit={handleSubmit} action="/submit" method="POST">
      {/* Enhanced form with loading states */}
    </form>
  )
}
```

### 18.3 Feature Detection Patterns

```tsx
// Safe feature detection
function ModernComponent() {
  const [supportsIntersectionObserver, setSupports] = useState(false)
  
  useEffect(() => {
    setSupports('IntersectionObserver' in window)
  }, [])
  
  if (!supportsIntersectionObserver) {
    return <FallbackComponent />
  }
  
  return <EnhancedComponent />
}
```

### 18.4 Network Resilience

```tsx
// Offline-first patterns
function DataComponent() {
  const { data, isLoading, error, isOffline } = useQuery({
    queryKey: ['data'],
    queryFn: fetchData,
    retry: (failureCount, error) => {
      // Don't retry if offline
      if (!navigator.onLine) return false
      return failureCount < 3
    },
    staleTime: 5 * 60 * 1000,
  })

  // Show cached data when offline
  if (isOffline && data) {
    return <CachedDataView data={data} isOffline />
  }
  
  // Normal loading/error states
  if (isLoading) return <LoadingState />
  if (error) return <ErrorState error={error} />
  
  return <DataView data={data} />
}
```

---

## 19. Security Patterns & Best Practices

### 19.1 Content Security Policy (CSP)

```tsx
// CSP-friendly component patterns
function SafeComponent() {
  // ❌ BAD: Inline styles and scripts
  const style = { color: 'red' }
  
  // ✅ GOOD: CSS classes and external scripts
  return (
    <div className="text-red-500">
      <script src="/trusted-script.js" async />
    </div>
  )
}
```

### 19.2 Input Sanitization & Validation

```tsx
// Comprehensive input validation
function SecureForm() {
  const [input, setInput] = useState('')
  
  const sanitizeInput = (value: string): string => {
    // Remove potentially dangerous characters
    return value
      .replace(/[<>]/g, '') // Remove HTML brackets
      .replace(/javascript:/gi, '') // Remove JS protocol
      .trim()
      .slice(0, 1000) // Limit length
  }
  
  const validateInput = (value: string): boolean => {
    // Business logic validation
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/
    return emailRegex.test(value)
  }
  
  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const sanitized = sanitizeInput(e.target.value)
    setInput(sanitized)
  }
  
  return (
    <input
      type="email"
      value={input}
      onChange={handleChange}
      required
      aria-invalid={!validateInput(input)}
    />
  )
}
```

### 19.3 Secure Data Handling

```tsx
// Secure API communication
function SecureDataComponent() {
  const { data, isLoading } = useQuery({
    queryKey: ['sensitive-data'],
    queryFn: async () => {
      const response = await fetch('/api/sensitive-data', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'X-CSRF-Token': getCSRFToken(), // CSRF protection
        },
        credentials: 'same-origin', // Include cookies
      })
      
      if (!response.ok) {
        throw new Error('Authentication failed')
      }
      
      return response.json()
    },
  })
  
  // Don't log sensitive data
  useEffect(() => {
    if (data) {
      console.log('Data loaded successfully') // ✅ Safe
      // console.log('User data:', data) // ❌ Unsafe
    }
  }, [data])
  
  return <DataDisplay data={data} isLoading={isLoading} />
}
```

### 19.4 Authentication & Authorization Patterns

```tsx
// Role-based access control
function ProtectedComponent({ requiredRole }: { requiredRole: string }) {
  const { user, isLoading } = useAuth()
  
  if (isLoading) return <LoadingSpinner />
  
  // Check authorization
  if (!user || !hasRole(user, requiredRole)) {
    return <UnauthorizedMessage />
  }
  
  return <SecureContent user={user} />
}

// Secure routing
function SecureRoute({ children, requiredRole }: RouteProps) {
  const { user, isAuthenticated } = useAuth()
  
  if (!isAuthenticated) {
    return <Navigate to="/login" replace />
  }
  
  if (requiredRole && !hasRole(user, requiredRole)) {
    return <Navigate to="/unauthorized" replace />
  }
  
  return <>{children}</>
}
```

---

## 20. Mobile-First & PWA Considerations

### 20.1 Mobile-First Responsive Design

```tsx
// Mobile-first breakpoint strategy
function ResponsiveGrid() {
  return (
    <div className="
      grid grid-cols-1 gap-4        /* Mobile: 1 column */
      sm:grid-cols-2 gap-6          /* Small: 2 columns */
      lg:grid-cols-3 gap-8          /* Large: 3 columns */
      xl:grid-cols-4 gap-10         /* Extra large: 4 columns */
    ">
      {items.map(item => (
        <ProductCard key={item.id} product={item} />
      ))}
    </div>
  )
}
```

### 20.2 Touch-Friendly Interactions

```tsx
// Touch-optimized components
function TouchButton({ children, onClick, ...props }) {
  return (
    <button
      onClick={onClick}
      className="
        min-h-[44px] min-w-[44px]        /* iOS touch target minimum */
        px-6 py-3                        /* Generous padding */
        active:scale-95                  /* Touch feedback */
        transition-transform duration-100
        focus:outline-none focus:ring-2  /* Keyboard accessible */
      "
      {...props}
    >
      {children}
    </button>
  )
}

// Swipe gestures support
function SwipeableCard({ onSwipeLeft, onSwipeRight, children }) {
  const [touchStart, setTouchStart] = useState(0)
  
  const handleTouchStart = (e: React.TouchEvent) => {
    setTouchStart(e.touches[0].clientX)
  }
  
  const handleTouchEnd = (e: React.TouchEvent) => {
    const touchEnd = e.changedTouches[0].clientX
    const diff = touchStart - touchEnd
    
    if (Math.abs(diff) > 50) { // Minimum swipe distance
      if (diff > 0) {
        onSwipeLeft?.()
      } else {
        onSwipeRight?.()
      }
    }
  }
  
  return (
    <div
      onTouchStart={handleTouchStart}
      onTouchEnd={handleTouchEnd}
      className="touch-pan-y"
    >
      {children}
    </div>
  )
}
```

### 20.3 PWA Features Implementation

```tsx
// Service Worker registration
function PWAInstaller() {
  const [deferredPrompt, setDeferredPrompt] = useState<any>(null)
  const [isInstallable, setIsInstallable] = useState(false)
  
  useEffect(() => {
    const handleBeforeInstallPrompt = (e: Event) => {
      e.preventDefault()
      setDeferredPrompt(e)
      setIsInstallable(true)
    }
    
    window.addEventListener('beforeinstallprompt', handleBeforeInstallPrompt)
    
    return () => {
      window.removeEventListener('beforeinstallprompt', handleBeforeInstallPrompt)
    }
  }, [])
  
  const handleInstall = async () => {
    if (!deferredPrompt) return
    
    deferredPrompt.prompt()
    const { outcome } = await deferredPrompt.userChoice
    
    setDeferredPrompt(null)
    setIsInstallable(false)
  }
  
  if (!isInstallable) return null
  
  return (
    <button onClick={handleInstall} className="install-btn">
      Install App
    </button>
  )
}

// Offline support with caching
function OfflineAwareComponent() {
  const [isOnline, setIsOnline] = useState(navigator.onLine)
  
  useEffect(() => {
    const handleOnline = () => setIsOnline(true)
    const handleOffline = () => setIsOnline(false)
    
    window.addEventListener('online', handleOnline)
    window.addEventListener('offline', handleOffline)
    
    return () => {
      window.removeEventListener('online', handleOnline)
      window.removeEventListener('offline', handleOffline)
    }
  }, [])
  
  if (!isOnline) {
    return <OfflineBanner />
  }
  
  return <OnlineContent />
}
```

---

## 21. Component Library & Documentation Standards

### 21.1 Component Library Structure

```tsx
// Standard component library pattern
export interface ComponentLibraryProps {
  /** Design system variant */
  variant?: 'primary' | 'secondary' | 'accent'
  /** Size preset */
  size?: 'xs' | 'sm' | 'md' | 'lg' | 'xl'
  /** Additional CSS classes */
  className?: string
  /** Test ID for testing */
  'data-testid'?: string
}

// Base component with consistent API
export function BaseComponent({
  variant = 'primary',
  size = 'md',
  className,
  'data-testid': testId,
  children,
  ...props
}: ComponentLibraryProps & React.HTMLAttributes<HTMLDivElement>) {
  const classes = cn(
    'base-component',
    `base-component--${variant}`,
    `base-component--${size}`,
    className
  )
  
  return (
    <div
      className={classes}
      data-testid={testId}
      {...props}
    >
      {children}
    </div>
  )
}
```

### 21.2 Documentation Standards

```tsx
/**
 * Button component with consistent design system integration.
 * 
 * @example
 * ```tsx
 * <Button variant="primary" size="lg" onClick={handleClick}>
 *   Click me
 * </Button>
 * ```
 */
export const Button = forwardRef<HTMLButtonElement, ButtonProps>(
  ({ variant = 'primary', size = 'md', children, ...props }, ref) => {
    return (
      <button
        ref={ref}
        className={buttonVariants({ variant, size })}
        {...props}
      >
        {children}
      </button>
    )
  }
)

Button.displayName = 'Button'

// Storybook stories for documentation
const meta: Meta<typeof Button> = {
  title: 'Components/Button',
  component: Button,
  parameters: {
    layout: 'centered',
    docs: {
      description: {
        component: 'A versatile button component with multiple variants and sizes.',
      },
    },
  },
  argTypes: {
    variant: {
      control: 'select',
      options: ['primary', 'secondary', 'ghost'],
    },
    size: {
      control: 'select',
      options: ['sm', 'md', 'lg'],
    },
  },
}
```

### 21.3 Design System Integration

```tsx
// Design token usage
export const tokens = {
  colors: {
    primary: {
      50: 'hsl(210, 100%, 98%)',
      500: 'hsl(210, 100%, 50%)',
      900: 'hsl(210, 100%, 10%)',
    },
  },
  spacing: {
    xs: '0.25rem',
    sm: '0.5rem',
    md: '1rem',
    lg: '1.5rem',
    xl: '2rem',
  },
  typography: {
    fontFamily: {
      sans: ['Inter', 'system-ui', 'sans-serif'],
      mono: ['JetBrains Mono', 'monospace'],
    },
  },
}

// Component using design tokens
function ThemedComponent() {
  return (
    <div style={{
      backgroundColor: 'var(--color-primary-50)',
      padding: 'var(--spacing-lg)',
      fontFamily: 'var(--font-family-sans)',
    }}>
      Content
    </div>
  )
}
```

---

## 22. Performance Monitoring & Observability

### 22.1 Performance Metrics Collection

```tsx
// Core Web Vitals monitoring
function PerformanceMonitor() {
  useEffect(() => {
    // Measure LCP (Largest Contentful Paint)
    const measureLCP = () => {
      new PerformanceObserver((entryList) => {
        const entries = entryList.getEntries()
        const lastEntry = entries[entries.length - 1]
        
        // Send to analytics
        analytics.track('lcp', {
          value: lastEntry.startTime,
          element: lastEntry.element?.tagName,
        })
      }).observe({ entryTypes: ['largest-contentful-paint'] })
    }
    
    // Measure FID (First Input Delay)
    const measureFID = () => {
      new PerformanceObserver((entryList) => {
        for (const entry of entryList.getEntries()) {
          analytics.track('fid', {
            value: entry.processingStart - entry.startTime,
          })
        }
      }).observe({ entryTypes: ['first-input'] })
    }
    
    // Measure CLS (Cumulative Layout Shift)
    const measureCLS = () => {
      let clsValue = 0
      
      new PerformanceObserver((entryList) => {
        for (const entry of entryList.getEntries()) {
          if (!entry.hadRecentInput) {
            clsValue += entry.value
          }
        }
        
        analytics.track('cls', { value: clsValue })
      }).observe({ entryTypes: ['layout-shift'] })
    }
    
    measureLCP()
    measureFID()
    measureCLS()
  }, [])
  
  return null
}
```

### 22.2 Component Performance Tracking

```tsx
// Component render performance tracking
function withPerformanceTracking<P extends object>(
  Component: React.ComponentType<P>,
  componentName: string
) {
  return function TrackedComponent(props: P) {
    const renderStart = useRef<number>()
    
    useLayoutEffect(() => {
      renderStart.current = performance.now()
    })
    
    useEffect(() => {
      if (renderStart.current) {
        const renderTime = performance.now() - renderStart.current
        
        // Track slow renders
        if (renderTime > 16) { // > 1 frame at 60fps
          console.warn(`Slow render: ${componentName} took ${renderTime.toFixed(2)}ms`)
          analytics.track('slow-render', {
            component: componentName,
            duration: renderTime,
          })
        }
      }
    })
    
    return <Component {...props} />
  }
}

// Usage
const ExpensiveComponent = withPerformanceTracking(HeavyChart, 'HeavyChart')
```

### 22.3 Error Boundaries with Monitoring

```tsx
// Error boundary with observability
class ErrorBoundary extends React.Component<
  { children: React.ReactNode; fallback?: React.ReactNode },
  { hasError: boolean; error?: Error }
> {
  constructor(props: any) {
    super(props)
    this.state = { hasError: false }
  }
  
  static getDerivedStateFromError(error: Error) {
    return { hasError: true, error }
  }
  
  componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
    // Send to error monitoring service
    errorReporting.captureException(error, {
      contexts: {
        react: {
          componentStack: errorInfo.componentStack,
        },
      },
      tags: {
        component: 'ErrorBoundary',
      },
    })
    
    // Log to console in development
    if (process.env.NODE_ENV === 'development') {
      console.error('Error caught by boundary:', error, errorInfo)
    }
  }
  
  render() {
    if (this.state.hasError) {
      return this.props.fallback || (
        <div className="error-fallback">
          <h2>Something went wrong</h2>
          <button onClick={() => this.setState({ hasError: false })}>
            Try again
          </button>
        </div>
      )
    }
    
    return this.props.children
  }
}
```

---

## 23. Framework-Specific Considerations

### React (General)
- Use functional components exclusively
- Leverage hooks for logic reuse
- Keep components small and focused

### Next.js
- Default to Server Components (RSC)
- Use Client Components (`'use client'`) only when needed
- Leverage built-in Image, Link, and Font optimization
- Implement proper metadata for SEO

### Vue 3
- Use Composition API over Options API
- Leverage `<script setup>` syntax
- Use `defineProps` and `defineEmits` for type safety
- Consider Pinia for state management

### Svelte
- Embrace reactivity system ($ prefix)
- Use stores for global state
- Leverage compile-time optimization
- Use `{#key}` blocks for conditional re-rendering

---

## End of Guide

**Remember**: The goal is to generate components that are:
- **Professional** — ready for real-world use
- **Accessible** — usable by everyone
- **Performant** — fast and efficient
- **Maintainable** — easy to understand and modify
- **Delightful** — pleasant to use with thoughtful micro-interactions

**When in doubt**: Favor simplicity, accessibility, and clarity over cleverness.

---

## Revision History
- **v2.0 (2025)**: Complete rewrite incorporating design systems, design tokens, micro-interactions, modern performance patterns, and accessibility standards
- **v1.0**: Initial version
