# Jest and React Testing Library

What needs to be tested:

1. **Structure:**
   1. do the elements are actually mounted? How about on subsequent rerender?
   2. do the number of elements mounted are correct?
2. **Usability:**
   1. Does the user able to use the app accordingly?
   2. Do functions work as intended?
3. **Error:**
   1. Is there a runtime error while rendering, fetching, or while using the app?

## Mocking

### Why mock a module / function?

The reason can be one of the following:

- It is not deterministic
- It is slow (ex: it performs I/O)
- We want to test only a single unit, hence other complementary units need to be mocked.

> Always cleanup after you mock a dependency, because other test might not need it to be mocked or might need to mock it in a different way

### Mock with Monkey Patching

Monkey patching is a technique to add, modify, or suppress the default behavior of a piece of code at runtime without changing its original source code.

```js
const foo = () => {};

// save the original function on a temporary variable
const originalFunction = foo;

foo = () => {
  // create a new implementation
  // we can also call the previous function
  originalFunction();
};

// make sure to cleanup if this monkey patching is for mocking
foo = originalFunction;
```

### Track JavaScript Function

Often when writing JavaScript tests and mocking dependencies, ==you’ll want to verify that the function was called correctly==. That requires:

- keeping track of how often the function was called
- what arguments it was called with

That way we can make assertions on how many times it was called and ensure it was called with the right arguments.

#### Track with `jest.fn()`

We can track JavaScript function using `jest.fn(implementation)`:

```js
test(('test example') => {
	const mockFn = jest.fn();
	mockFn();
	expect(mockFn).toHaveBeenCalled();

	// With a mock implementation:
	const returnsTrue = jest.fn(() => true);
	console.log(returnsTrue()); // true;
})
```

#### Track and mock with `jest.spyOn()`

We can also track JavaScript function using `jest.spyOn(object, methodName)`.

Additionally, it is better suited for mocking. This API is still considered as monkey patching.

```js
test(('test example') => {
	const foo = {
		bar: () => {}
	};

	jest.spyOn(foo, 'bar');
	foo.bar.mockImplementation(() => {
		// do something
	});

	// cleanup
	foo.bar.mockRestore();
})
```

### Mock with `jest.mock`

`jest.mock(moduleName, factory, options)`
