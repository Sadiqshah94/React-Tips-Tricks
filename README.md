# React-Tips-Tricks 
by **Ndeye Fatou Diop**
## Components organization 🧹

### 1. Use self-closing tags to keep your code compact

```
// ❌ Bad: too verbose
<MyComponent></MyComponent>

// ✅ Good
<MyComponent/>

```

### 2. Prefer fragments over DOM nodes (e.g., div, span, etc.) to group elements

```
//❌ Bad: Using div clutters your DOM and may require more CSS code.

function Dashboard() {
  return (
    <div>
      <Header />
      <Main />
    </div>
  );
}

//✅ Good: <Fragment> wraps elements without affecting the DOM structure.

function Dashboard() {
  return (
    <Fragment>
      <Header />
      <Main />
    </Fragment>
  );
}
```

### 3. Use React fragment shorthand <></> (except if you need to set a key)
````
❌ Bad: The code below is unnecessarily verbose.

<Fragment>
   <FirstChild />
   <SecondChild />
</Fragment>
✅ Good: Unless, you need a key, <></> is more concise.

<>
   <FirstChild />
   <SecondChild />
</>

// Using a `Fragment` here is required because of the key.
function List({ users }) {
  return (
    <div>
      {users.map((user) => (
        <Fragment key={user.id}>
          <span>{user.name}</span>
          <span>{user.occupation}</span>
        </Fragment>
      ))}
    </div>
  );
}
````

### 4. Prefer spreading props over accessing each one individually
````
❌ Bad: The code below is harder to read (especially at scale).

// We do `props…` all over the code.
function TodoList(props) {
  return (
    <div>
      {props.todos.map((todo) => (
        <div key={todo}>
          <button
            onClick={() => props.onSelectTodo(todo)}
            style={{
              backgroundColor: todo === props.selectedTodo ? "gold" : undefined,
            }}
          >
            <span>{todo}</span>
          </button>
        </div>
      ))}
    </div>
  );
}
✅ Good: The code below is more concise.

function TodoList({ todos, selectedTodo, onSelectTodo }) {
  return (
    <div>
      {todos.map((todo) => (
        <div key={todo}>
          <button
            onClick={() => onSelectTodo(todo)}
            style={{
              backgroundColor: todo === selectedTodo ? "gold" : undefined,
            }}
          >
            <span>{todo}</span>
          </button>
        </div>
      ))}
    </div>
  );
}
````

### 5. When setting default values for props, do it while destructuring them
````
❌ Bad: You may need to define the defaults in multiple places and introduce new variables.

function Button({ onClick, text, small, colorScheme }) {
  let scheme = colorScheme || "light";
  let isSmall = small || false;
  return (
    <button
      onClick={onClick}
      style={{
        color: scheme === "dark" ? "white" : "black",
        fontSize: isSmall ? "12px" : "16px",
      }}
    >
      {text ?? "Click here"}
    </button>
  );
}
✅ Good: You can set all your defaults in one place at the top. This makes it easy for someone to locate them.

function Button({
  onClick,
  text = "Click here",
  small = false,
  colorScheme = "light",
}) {
  return (
    <button
      onClick={onClick}
      style={{
        color: colorScheme === "dark" ? "white" : "black",
        fontSize: small ? "12px" : "16px",
      }}
    >
      {text}
    </button>
  );
}
````


### 6. Drop curly braces when passing string type props.
````
// ❌ Bad: curly braces are not needed
<Button text={"Click me"} colorScheme={"dark"} />

// ✅ Good
<Button text="Click me" colorScheme="dark" />
````

### 7. Ensure that value is a boolean before using value && <Component {...props}/> to prevent displaying unexpected values on the screen.
````
❌ Bad: When the list is empty, 0 will be printed on the screen.

export function ListWrapper({ items, selectedItem, setSelectedItem }) {
  return (
    <div className="list">
      {items.length && ( // `0` if the list is empty
        <List
          items={items}
          onSelectItem={setSelectedItem}
          selectedItem={selectedItem}
        />
      )}
    </div>
  );
}

✅ Good: Nothing will be printed on the screen when there are no items.

export function ListWrapper({ items, selectedItem, setSelectedItem }) {
  return (
    <div className="list">
      {items.length > 0 && (
        <List
          items={items}
          onSelectItem={setSelectedItem}
          selectedItem={selectedItem}
        />
      )}
    </div>
  );
}
````

8. Use functions (inline or not) to avoid polluting your scope with intermediate variables

````
❌ Bad: The variables gradeSum and gradeCount are cluttering the component's scope.

function Grade({ grades }) {
  if (grades.length === 0) {
    return <>No grades available.</>;
  }

  let gradeSum = 0;
  let gradeCount = 0;

  grades.forEach((grade) => {
    gradeCount++;
    gradeSum += grade;
  });

  const averageGrade = gradeSum / gradeCount;

  return <>Average Grade: {averageGrade}</>;
}

✅ Good: The variables gradeSum and gradeCountare scoped within computeAverageGrade function.

function Grade({ grades }) {
  if (grades.length === 0) {
    return <>No grades available.</>;
  }

  const computeAverageGrade = () => {
    let gradeSum = 0;
    let gradeCount = 0;
    grades.forEach((grade) => {
      gradeCount++;
      gradeSum += grade;
    });
    return gradeSum / gradeCount;
  };

  return <>Average Grade: {computeAverageGrade()}</>;
}
````



