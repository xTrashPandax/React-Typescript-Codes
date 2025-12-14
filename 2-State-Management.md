## STATE MANAGEMENT (The Brain of React)

If UI components are the *body*, **state is the nervous system**.
Most React confusion disappears once this is internalized.

---

## A. WHAT STATE IS (MENTAL MODEL)

* **State = memory**
* UI is a **pure reflection of state**
* Change state → React re-renders → UI updates

**Golden rule**

> Never manually update the UI.
> Update state and let React do the rest.

---

## B. BASIC STATE (`useState`)

```tsx
const [count, setCount] = useState(0);

<button onClick={() => setCount(count + 1)}>
  {count}
</button>
```

**Rules**

* State updates are asynchronous
* Never mutate state directly
* Always use the setter

---

## C. STATE WITH OBJECTS (MOST COMMON)

```tsx
const [user, setUser] = useState({
  name: "",
  email: "",
});
```

Update safely:

```tsx
setUser({
  ...user,
  name: "John",
});
```

**Rule**

* Always spread the previous state
* Never overwrite accidentally

---

## D. STATE WITH ARRAYS

```tsx
const [items, setItems] = useState<string[]>([]);
```

Add item:

```tsx
setItems([...items, "New Item"]);
```

Remove item:

```tsx
setItems(items.filter(i => i !== "Old Item"));
```

**Rule**

* Treat arrays as immutable

---

## E. DERIVED STATE (DO NOT STORE THIS)

Bad:

```tsx
const [total, setTotal] = useState(0);
```

Good:

```tsx
const total = items.length;
```

**Rule**

> If it can be calculated from existing state,
> **do not store it in state**.

---

## F. MULTIPLE STATES VS SINGLE STATE

Acceptable:

```tsx
const [email, setEmail] = useState("");
const [password, setPassword] = useState("");
```

Also acceptable:

```tsx
const [form, setForm] = useState({ email: "", password: "" });
```

**Guideline**

* Small forms → multiple states
* Large forms → single object

---

## G. PREVIOUS STATE UPDATES (IMPORTANT)

Wrong:

```tsx
setCount(count + 1);
```

Correct:

```tsx
setCount(prev => prev + 1);
```

Use this when:

* New state depends on old state
* Multiple updates may happen quickly

---

## H. CONDITIONAL STATE-DRIVEN UI

```tsx
const [loading, setLoading] = useState(false);

{loading && <p>Loading...</p>}
```

UI logic always follows state.

---

## I. LIFTING STATE UP (VERY IMPORTANT CONCEPT)

Child needs data → Parent owns state.

```tsx
function Parent() {
  const [value, setValue] = useState("");

  return <Child value={value} onChange={setValue} />;
}

function Child({ value, onChange }: any) {
  return (
    <input
      value={value}
      onChange={(e) => onChange(e.target.value)}
    />
  );
}
```

**Rule**

* State lives at the lowest common parent

---

## J. SHARED STATE (WITHOUT LIBRARIES)

```tsx
const AppContext = createContext(null);

function AppProvider({ children }: any) {
  const [user, setUser] = useState(null);

  return (
    <AppContext.Provider value={{ user, setUser }}>
      {children}
    </AppContext.Provider>
  );
}
```

Consume:

```tsx
const { user, setUser } = useContext(AppContext);
```

Use this for:

* Logged-in user
* Theme
* Global settings

---

## K. WHAT NOT TO PUT IN STATE

Do NOT store:

* DOM elements
* Computed values
* Props copied into state
* Constants

---

## L. COMMON BEGINNER MISTAKES

* Mutating state directly
* Storing derived data
* Too much global state
* Using state when props are enough

---

## M. PRACTICE EXERCISE (IMPORTANT)

Build a page with:

* Counter
* List of items
* Add/remove item buttons
* Loading flag that disables buttons

No APIs. No routing.

If you can do this cleanly, **React clicks**.

---

## STATE IN ONE SENTENCE

> State is the single source of truth.
> UI is just a mirror.
