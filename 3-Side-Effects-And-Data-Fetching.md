## SIDE EFFECTS & DATA FETCHING (`useEffect`)

This is the bridge between **React UI** and the **outside world**
(APIs, timers, subscriptions, browser events).

If state is the brain, **effects are the senses**.

---

## A. WHAT A SIDE EFFECT IS

A side effect is **anything that React cannot calculate during render**.

Examples:

* Fetching data
* Calling an API
* Setting timers
* Subscribing to events
* Reading from storage

**Rule**

> Rendering must be pure.
> Side effects go in `useEffect`.

---

## B. BASIC `useEffect` SHAPE

```tsx
useEffect(() => {
  console.log("Effect ran");
}, []);
```

The `[]` is the **dependency array**.

---

## C. WHEN `useEffect` RUNS

```tsx
useEffect(() => {
  // runs once (on mount)
}, []);
```

```tsx
useEffect(() => {
  // runs when `value` changes
}, [value]);
```

```tsx
useEffect(() => {
  // runs on every render (rarely correct)
});
```

**99% of the time you want one of the first two.**

---

## D. FETCHING DATA (MOST COMMON USE)

```tsx
const [data, setData] = useState(null);
const [loading, setLoading] = useState(true);
const [error, setError] = useState<string | null>(null);

useEffect(() => {
  fetch("/api/users")
    .then(res => res.json())
    .then(result => {
      setData(result);
      setLoading(false);
    })
    .catch(() => {
      setError("Failed to load");
      setLoading(false);
    });
}, []);
```

**Pattern**

* loading → data OR error
* Always handle all three states

---

## E. CONDITIONAL RENDERING WITH EFFECT DATA

```tsx
if (loading) return <p>Loading...</p>;
if (error) return <p>{error}</p>;

return <div>{data.name}</div>;
```

This is professional-grade UI behavior.

---

## F. DEPENDENCY RULES (VERY IMPORTANT)

```tsx
useEffect(() => {
  fetchData(userId);
}, [userId]);
```

**Rule**

> Every variable used inside the effect
> that can change must be in the dependency array.

Do NOT fight this rule. It prevents bugs.

---

## G. CLEANUP FUNCTION

Used for:

* Timers
* Subscriptions
* Event listeners

```tsx
useEffect(() => {
  const timer = setInterval(() => {
    console.log("tick");
  }, 1000);

  return () => clearInterval(timer);
}, []);
```

**Rule**

> If you start something, clean it up.

---

## H. ASYNC IN `useEffect` (CORRECT WAY)

Wrong:

```tsx
useEffect(async () => { ... }, []);
```

Correct:

```tsx
useEffect(() => {
  async function load() {
    const res = await fetch("/api/data");
    const json = await res.json();
    setData(json);
  }

  load();
}, []);
```

---

## I. AVOIDING INFINITE LOOPS

Bad:

```tsx
useEffect(() => {
  setCount(count + 1);
}, [count]);
```

Why:

* Effect changes state
* State triggers effect
* Infinite loop

**Fix**

* Rethink logic
* Or use conditional guards

---

## J. MULTIPLE EFFECTS (BEST PRACTICE)

```tsx
useEffect(() => {
  fetchUser();
}, []);

useEffect(() => {
  logAnalytics(page);
}, [page]);
```

**Rule**

* One effect = one responsibility

---

## K. EFFECT VS EVENT HANDLER

| Event Handler | useEffect       |
| ------------- | --------------- |
| User clicks   | Component loads |
| User types    | State changes   |
| Button press  | External sync   |

**Rule**

> If it’s caused by a user action → handler
> If it’s caused by rendering/state → effect

---

## L. PRACTICE EXERCISE

Build a page that:

* Fetches data on load
* Shows loading
* Shows error
* Displays list on success
* Refetches when a button is clicked

No routing yet.

---

## `useEffect` IN ONE SENTENCE

> `useEffect` synchronizes your component
> with the outside world.
