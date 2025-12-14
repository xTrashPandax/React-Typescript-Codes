## LISTS, TABLES & REPEATING UI

Most business applications are **data-driven**. Understanding lists and tables is **essential**.

---

## A. BASIC LIST RENDERING (`.map()`)

```tsx
const items = ["Apple", "Banana", "Cherry"];

<ul>
  {items.map((item, index) => (
    <li key={index}>{item}</li>
  ))}
</ul>
```

**Rules**

* Always provide a `key` (unique identifier preferred)
* Do not use index as key if list changes frequently

---

## B. TABLES

```tsx
const users = [
  { id: 1, name: "John" },
  { id: 2, name: "Jane" },
];

<table>
  <thead>
    <tr>
      <th>ID</th>
      <th>Name</th>
    </tr>
  </thead>
  <tbody>
    {users.map(user => (
      <tr key={user.id}>
        <td>{user.id}</td>
        <td>{user.name}</td>
      </tr>
    ))}
  </tbody>
</table>
```

**Tips**

* Use `thead` and `tbody` for semantic HTML
* Keep row rendering minimal for performance

---

## C. CONDITIONAL LISTS

```tsx
{items.length === 0 ? (
  <p>No items available</p>
) : (
  <ul>
    {items.map(item => <li key={item.id}>{item.name}</li>)}
  </ul>
)}
```

Always handle **empty states**.

---

## D. FILTERING LISTS

```tsx
const filtered = items.filter(item => item.includes(searchTerm));
```

Rule:

* Do not mutate the original array
* Keep state for search term, not filtered array

---

## E. SORTING LISTS

```tsx
const sorted = [...items].sort((a, b) => a.name.localeCompare(b.name));
```

**Rule**

* Spread first to avoid mutating original state

---

## F. PAGINATION (BASIC)

```tsx
const itemsPerPage = 5;
const [page, setPage] = useState(1);

const pagedItems = items.slice(
  (page - 1) * itemsPerPage,
  page * itemsPerPage
);
```

Add navigation:

```tsx
<button onClick={() => setPage(page - 1)} disabled={page === 1}>
  Prev
</button>
<button onClick={() => setPage(page + 1)} disabled={page === maxPage}>
  Next
</button>
```

---

## G. TABLE ACTIONS (EDIT / DELETE)

```tsx
<tr key={user.id}>
  <td>{user.name}</td>
  <td>
    <button onClick={() => editUser(user.id)}>Edit</button>
    <button onClick={() => deleteUser(user.id)}>Delete</button>
  </td>
</tr>
```

**Pattern**

* Buttons inside each row trigger operations
* Use row ID as reference

---

## H. PERFORMANCE TIP

```tsx
{users.map(user => (
  <UserRow key={user.id} user={user} />
))}

// In UserRow, use React.memo if props are stable
```

Large lists → optimize with **memoization** or libraries like `react-window`.

---

## I. PRACTICE EXERCISE

Build a **user dashboard**:

* Table with name + email + actions
* Sort by name
* Filter by search
* Pagination
* Delete action

This is **the core of document/approval systems**.

---

## LISTS & TABLES IN ONE SENTENCE

> Lists display arrays, tables organize structured data, and conditional logic + actions make them interactive.
