## API COMMUNICATION PATTERNS (GET / POST / PUT / DELETE)

This cheat sheet teaches you **how real applications talk to servers** in a clean, repeatable, professional way.

Forms + state + effects **mean nothing** unless data flows correctly.

---

## A. THE API REQUEST LIFECYCLE (MEMORIZE THIS)

Every request has **four phases**:

1. Start request
2. Set loading state
3. Handle success
4. Handle error

If any phase is missing, the UI will break.

---

## B. BASIC GET REQUEST

```tsx
const [data, setData] = useState<any[]>([]);
const [loading, setLoading] = useState(false);
const [error, setError] = useState<string | null>(null);

const loadData = async () => {
  setLoading(true);
  setError(null);

  try {
    const res = await fetch("/api/items");
    if (!res.ok) throw new Error("Request failed");
    const json = await res.json();
    setData(json);
  } catch (err) {
    setError("Failed to load data");
  } finally {
    setLoading(false);
  }
};
```

Call it:

```tsx
useEffect(() => {
  loadData();
}, []);
```

---

## C. POST REQUEST (CREATE)

```tsx
const createItem = async (item: any) => {
  setLoading(true);

  try {
    const res = await fetch("/api/items", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(item),
    });

    if (!res.ok) throw new Error();
    await loadData(); // refresh list
  } catch {
    alert("Create failed");
  } finally {
    setLoading(false);
  }
};
```

**Pattern**

* Submit form
* POST data
* Refresh view

---

## D. PUT REQUEST (UPDATE)

```tsx
const updateItem = async (id: number, item: any) => {
  await fetch(`/api/items/${id}`, {
    method: "PUT",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(item),
  });

  await loadData();
};
```

---

## E. DELETE REQUEST

```tsx
const deleteItem = async (id: number) => {
  if (!window.confirm("Are you sure?")) return;

  await fetch(`/api/items/${id}`, {
    method: "DELETE",
  });

  setData(data.filter(item => item.id !== id));
};
```

**Tip**

* Optimistic update improves UX

---

## F. HANDLING LOADING STATES

```tsx
<button disabled={loading}>
  {loading ? "Saving..." : "Save"}
</button>
```

Never allow double submissions.

---

## G. HANDLING ERRORS CLEANLY

```tsx
{error && <p style={{ color: "red" }}>{error}</p>}
```

Errors must be:

* Visible
* Clear
* Recoverable

---

## H. MAPPING API DATA TO UI

```tsx
const users = data.map((u: any) => ({
  id: u.id,
  name: u.first_name,
}));
```

**Rule**

* Do not trust API shape blindly
* Map once, use everywhere

---

## I. CENTRALIZING API CALLS (IMPORTANT)

```tsx
// api.ts
export async function getUsers() {
  const res = await fetch("/api/users");
  return res.json();
}
```

Use in components:

```tsx
const users = await getUsers();
```

This prevents chaos as the app grows.

---

## J. AUTH HEADERS (PREVIEW)

```tsx
fetch("/api/data", {
  headers: {
    Authorization: `Bearer ${token}`,
  },
});
```

Full auth comes later.

---

## K. COMMON API MISTAKES

* No loading state
* Silent failures
* Mixing UI and API logic
* Repeating fetch code everywhere

---

## L. PRACTICE EXERCISE

Build a page with:

* List loaded via GET
* Create item via POST
* Delete item via DELETE
* Loading + error handling

This is **80% of business applications**.

---

## API COMMUNICATION IN ONE SENTENCE

> UI requests data, reacts to outcomes, and never assumes success.
