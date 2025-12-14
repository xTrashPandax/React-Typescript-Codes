# A1. BUTTON TEMPLATES

## 1. Basic Button (Reusable)

```tsx
type ButtonProps = {
  label: string;
  onClick?: () => void;
  type?: "button" | "submit";
  disabled?: boolean;
};

export function Button({
  label,
  onClick,
  type = "button",
  disabled = false,
}: ButtonProps) {
  return (
    <button type={type} onClick={onClick} disabled={disabled}>
      {label}
    </button>
  );
}
```

---

## 2. Button Variants (Primary / Secondary / Danger)

```tsx
type Variant = "primary" | "secondary" | "danger";

type ButtonProps = {
  label: string;
  variant?: Variant;
};

export function Button({ label, variant = "primary" }: ButtonProps) {
  return <button className={`btn ${variant}`}>{label}</button>;
}
```

---

# A2. TEXT & DISPLAY COMPONENTS

## 1. Heading

```tsx
type HeadingProps = {
  level?: 1 | 2 | 3 | 4;
  text: string;
};

export function Heading({ level = 1, text }: HeadingProps) {
  const Tag = `h${level}` as keyof JSX.IntrinsicElements;
  return <Tag>{text}</Tag>;
}
```

---

## 2. Label + Helper + Error

```tsx
type FieldTextProps = {
  label?: string;
  helper?: string;
  error?: string;
};

export function FieldText({ label, helper, error }: FieldTextProps) {
  return (
    <div>
      {label && <label>{label}</label>}
      {helper && <small>{helper}</small>}
      {error && <p style={{ color: "red" }}>{error}</p>}
    </div>
  );
}
```

---

# A3. LAYOUT COMPONENTS

## 1. Container

```tsx
type Props = {
  children: React.ReactNode;
};

export function Container({ children }: Props) {
  return <div style={{ padding: "16px" }}>{children}</div>;
}
```

---

## 2. Flex Row / Column

```tsx
type FlexProps = {
  children: React.ReactNode;
  gap?: number;
};

export function Row({ children, gap = 8 }: FlexProps) {
  return <div style={{ display: "flex", gap }}>{children}</div>;
}

export function Column({ children, gap = 8 }: FlexProps) {
  return (
    <div style={{ display: "flex", flexDirection: "column", gap }}>
      {children}
    </div>
  );
}
```

---

## 3. Card / Panel

```tsx
type CardProps = {
  title?: string;
  children: React.ReactNode;
};

export function Card({ title, children }: CardProps) {
  return (
    <div style={{ border: "1px solid #ccc", padding: 16 }}>
      {title && <h3>{title}</h3>}
      {children}
    </div>
  );
}
```

---

# A4. NAVIGATION COMPONENTS

## 1. Navbar

```tsx
type NavItem = {
  label: string;
  onClick: () => void;
};

type NavbarProps = {
  items: NavItem[];
};

export function Navbar({ items }: NavbarProps) {
  return (
    <nav>
      {items.map((item) => (
        <button key={item.label} onClick={item.onClick}>
          {item.label}
        </button>
      ))}
    </nav>
  );
}
```

---

## 2. Tabs

```tsx
type TabProps = {
  tabs: string[];
  active: string;
  onChange: (tab: string) => void;
};

export function Tabs({ tabs, active, onChange }: TabProps) {
  return (
    <div>
      {tabs.map((tab) => (
        <button
          key={tab}
          onClick={() => onChange(tab)}
          style={{ fontWeight: tab === active ? "bold" : "normal" }}
        >
          {tab}
        </button>
      ))}
    </div>
  );
}
```

---

## 3. Breadcrumbs

```tsx
type Crumb = {
  label: string;
  onClick?: () => void;
};

export function Breadcrumbs({ items }: { items: Crumb[] }) {
  return (
    <div>
      {items.map((c, i) => (
        <span key={i}>
          <span onClick={c.onClick}>{c.label}</span>
          {i < items.length - 1 && " / "}
        </span>
      ))}
    </div>
  );
}
```

---

# A5. HOW THESE FIT TOGETHER (IMPORTANT)

A real page usually looks like this:

```tsx
<Container>
  <Navbar items={[...]} />
  <Card title="User Details">
    <Column>
      <Heading level={2} text="Profile" />
      <Button label="Save" />
    </Column>
  </Card>
</Container>
```

This is **professional composition**.
