# React-Tips-Tricks by **Ndeye Fatou Diop**

## Components organization 🧹

### 1. Use self-closing tags to keep your code compact

```
// ❌ Bad: too verbose
<MyComponent></MyComponent>

// ✅ Good
<MyComponent/>

```

2. Prefer fragments over DOM nodes (e.g., div, span, etc.) to group elements

```❌ Bad: Using div clutters your DOM and may require more CSS code.

function Dashboard() {
  return (
    <div>
      <Header />
      <Main />
    </div>
  );
}
```
