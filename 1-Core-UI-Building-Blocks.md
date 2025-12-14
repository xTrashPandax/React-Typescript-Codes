### CORE UI BUILDING BLOCKS (Beyond Forms)

This is the **foundation layer** everything else depends on.

---

### A. BUTTONS

```tsx
<button>Default</button>
<button disabled>Disabled</button>
<button type="submit">Submit</button>
```

With loading state:

```tsx
<button disabled={loading}>
  {loading ? "Saving..." : "Save"}
</button>
```

Rule:

* Buttons trigger actions
* Forms handle submission logic

---

### B. TEXT ELEMENTS

```tsx
<h1>Main Title</h1>
<h2>Section Title</h2>
<p>Description text</p>
<small>Helper text</small>
<span>Error message</span>
```

Rule:

* Titles structure pages
* Text explains intent
* Errors are always visible and near inputs

---

### C. LAYOUT (FLEXBOX – MOST USED)

```tsx
<div style={{ display: "flex", gap: "8px" }}>
  <div>Left</div>
  <div>Right</div>
</div>
```

Vertical stack:

```tsx
<div style={{ display: "flex", flexDirection: "column" }}>
  <div>Top</div>
  <div>Bottom</div>
</div>
```

Rule:

* Layout components should NOT contain business logic

---

### D. CONTAINER / PAGE WRAPPER

```tsx
function Page({ children }: { children: React.ReactNode }) {
  return <div style={{ padding: "16px" }}>{children}</div>;
}
```

Used everywhere.

---

### E. CARD / PANEL

```tsx
<div style={{ border: "1px solid #ccc", padding: "12px" }}>
  <h3>Title</h3>
  <p>Content</p>
</div>
```

Rule:

* Cards group related data
* One purpose per card

---

### F. NAVIGATION (SIMPLE)

```tsx
<nav>
  <a href="/">Home</a>
  <a href="/users">Users</a>
</nav>
```

Later this becomes router-based.

---

### G. CONDITIONAL DISPLAY (VERY IMPORTANT)

```tsx
{isLoading && <p>Loading...</p>}
{error && <p>Error occurred</p>}
{data && <div>{data.name}</div>}
```

Rule:

* UI reflects state
* Never assume data exists

---

### H. EMPTY STATES

```tsx
{items.length === 0 && <p>No data yet</p>}
```

Professional apps always have these.

---

## WHAT YOU SHOULD PRACTICE FROM THIS CHEAT SHEET

* Build a page using only:

  * Buttons
  * Text
  * Cards
  * Layout
* No APIs yet
* No forms yet

This trains **visual structure and discipline**.
