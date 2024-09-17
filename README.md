# JestUnitTestNotes

## Jest unit test on UI is hard
After 2 years of writing unit test in Jest, one might still find it diffcult to test the UI.
Well, I am not talking about testing the logics of some functions but DOM assertion.
In Jest's case, it is actually asserting the virtual DOM.

The real problem is that you cannot see the virtual DOM properly:
- When you log out DOM element into the console, it is either incomplete or truncated for some reasons
- When you cannot see it clearly, how could you write good unit tests
- The issue is worsened when your UI is very interactive, wrapped together with some API calls (well mocking API is already diffcult)

## Some ways to see the virtual DOM clearly

### Load the virtual DOM into a browser to see the UI layout.
Below will print out a URL in the terminal and you could load this into a browser to see the actual UI instead of JSX.
However, if there are too many logs coming from the test scripts, you might not see this URL due to the aforementioned truncation.

```
import { screen } from "@testing-library/dom";
screen.logTestingPlaygroundURL();
```

### Select a specific component to view in the terminal
You can also print out the component in HTML format in the terminal using below function.

```
screen.debug(<component/>)
```

## What is the difference between null and undefined in virtual DOM assertion
Sure! Here's a simpler explanation of the difference between `null` and `undefined` in virtual DOM testing:

In **Jest** testing with `@testing-library/react`, the matcher **`toBeInTheDocument()`** is used to check if an element **exists** in the DOM (virtual DOM in this case). It confirms that the element is present and part of the DOM tree.

### Key Differences Between `toBeInTheDocument()` and Other Matchers:

1. **`toBeInTheDocument()`**:
   - **Means:** The element **exists** and is part of the DOM.
   - **Used when:** You want to verify that an element is actually rendered in the DOM.
   - **Example:** You can use this when checking if a button or text is present on the page after rendering a component.
     ```javascript
     const button = screen.getByText('Submit');
     expect(button).toBeInTheDocument(); // The button exists in the DOM
     ```

2. **`toBeNull()`**:
   - **Means:** The element **does not exist** in the DOM.
   - **Used when:** You want to check that something is missing or wasn’t rendered.
   - **Example:** If a button isn't rendered, you'd expect `null`.
     ```javascript
     const button = screen.queryByText('Submit');
     expect(button).toBeNull(); // The button is not in the DOM
     ```

3. **`toBeUndefined()`**:
   - **Means:** A variable or query hasn't been assigned a value, or you didn't find an element with a `getBy*` query.
   - **Used when:** You haven’t assigned or queried the element yet.
   - **Example:** A variable that hasn’t been initialized would be `undefined`.
     ```javascript
     let button;
     expect(button).toBeUndefined(); // The button variable is undefined
     ```

### When to Use `toBeInTheDocument()`:
- You use `toBeInTheDocument()` when you expect that the element **should be present** after rendering or interacting with the component.
  
  For example, when you render a component that includes a button, you can check if the button is correctly rendered like this:

  ```javascript
  render(<MyComponent />);
  const button = screen.getByText('Submit');
  expect(button).toBeInTheDocument(); // This ensures the button is in the virtual DOM
  ```

### Summary:
- **`toBeInTheDocument()`**: Checks that an element **exists** in the DOM.
- **`toBeNull()`**: Checks that an element **doesn’t exist** in the DOM.
- **`toBeUndefined()`**: Checks that a variable or query result hasn’t been assigned a value.

So, **`toBeInTheDocument()`** is the go-to matcher when you want to assert that an element is properly rendered and part of the DOM structure in your tests.
