# JavaScript DOM API Cheat Sheet

A practical reference to important JavaScript DOM methods and properties for frontend development and interviews.

## Table of Contents

- [1. Selecting Elements](#1-selecting-elements)
- [2. DOM Traversal](#2-dom-traversal)
- [3. Creating and Modifying Elements](#3-creating-and-modifying-elements)
- [4. Reading and Updating Content](#4-reading-and-updating-content)
- [5. Attributes and Classes](#5-attributes-and-classes)
- [6. Event Handling](#6-event-handling)
- [7. Styles, Dimensions, and Scrolling](#7-styles-dimensions-and-scrolling)
- [8. Forms and Inputs](#8-forms-and-inputs)
- [9. Document and Window APIs](#9-document-and-window-apis)
- [10. Collections and Node Utilities](#10-collections-and-node-utilities)
- [11. Observers and Advanced APIs](#11-observers-and-advanced-apis)
- [12. Important Differences](#12-important-differences)
- [13. Practice Examples](#13-practice-examples)
- [14. Interview Preparation Checklist](#14-interview-preparation-checklist)

---

## 1. Selecting Elements

| Method / Property | Purpose |
|---|---|
| `document.getElementById(id)` | Select an element by its ID |
| `document.getElementsByClassName(name)` | Get elements with a class name |
| `document.getElementsByTagName(name)` | Get elements by tag name |
| `document.querySelector(selector)` | Get the first element matching a CSS selector |
| `document.querySelectorAll(selector)` | Get all elements matching a CSS selector |
| `element.matches(selector)` | Check whether an element matches a CSS selector |
| `element.closest(selector)` | Find the closest matching ancestor, including the element itself |

```js
const heading = document.querySelector("h1");
const cards = document.querySelectorAll(".card");
const button = document.getElementById("submit");
const card = button?.closest(".card");
```

## 2. DOM Traversal

Use traversal APIs to move through parent, child, and sibling nodes.

| Property / Method | Purpose |
|---|---|
| `element.parentElement` | Get the parent element, or `null` |
| `node.parentNode` | Get the parent node |
| `element.children` | Get direct child elements |
| `node.childNodes` | Get all child nodes, including text and comments |
| `element.firstElementChild` | Get the first child element |
| `element.lastElementChild` | Get the last child element |
| `node.firstChild` | Get the first child node |
| `node.lastChild` | Get the last child node |
| `element.nextElementSibling` | Get the next sibling element |
| `element.previousElementSibling` | Get the previous sibling element |
| `node.nextSibling` | Get the next sibling node |
| `node.previousSibling` | Get the previous sibling node |
| `element.childElementCount` | Count direct child elements |
| `node.hasChildNodes()` | Check whether a node has child nodes |
| `node.contains(otherNode)` | Check whether a node contains another node |

### Traversal example

HTML:

```html
<ul id="menu">
  <li>Home</li>
  <li>About</li>
  <li>Contact</li>
</ul>
```

JavaScript:

```js
const menu = document.querySelector("#menu");
const firstItem = menu.firstElementChild;

console.log(menu.children.length); // 3
console.log(firstItem.textContent); // "Home"
console.log(firstItem.nextElementSibling.textContent); // "About"
console.log(menu.lastElementChild.textContent); // "Contact"
console.log(firstItem.parentElement === menu); // true
```

**Interview tip:** Prefer `children`, `firstElementChild`, and `nextElementSibling` when you specifically want elements. The node-based alternatives may return whitespace text nodes or comments.

## 3. Creating and Modifying Elements

| Method | Purpose |
|---|---|
| `document.createElement(tag)` | Create an element |
| `document.createTextNode(text)` | Create a text node |
| `document.createDocumentFragment()` | Create a fragment to assemble nodes before insertion |
| `parent.append(...nodesOrStrings)` | Append nodes or text |
| `parent.appendChild(node)` | Append a node and return it |
| `parent.prepend(...nodesOrStrings)` | Insert nodes or text at the beginning |
| `parent.insertBefore(newNode, referenceNode)` | Insert a node before another child |
| `element.before(...nodesOrStrings)` | Insert content before an element |
| `element.after(...nodesOrStrings)` | Insert content after an element |
| `element.replaceWith(...nodesOrStrings)` | Replace an element |
| `parent.replaceChild(newNode, oldNode)` | Replace a child node |
| `element.remove()` | Remove an element |
| `parent.removeChild(child)` | Remove and return a child node |
| `element.cloneNode(deep)` | Clone a node; `true` also clones descendants |
| `element.replaceChildren(...nodesOrStrings)` | Replace all children |
| `element.insertAdjacentElement(position, newElement)` | Insert an element at a specified position |
| `element.insertAdjacentHTML(position, html)` | Insert parsed HTML at a specified position |
| `element.insertAdjacentText(position, text)` | Insert text at a specified position |

```js
const list = document.querySelector("#list");
const item = document.createElement("li");

item.textContent = "New item";
list.append(item);
```

`insertAdjacent...` positions are `"beforebegin"`, `"afterbegin"`, `"beforeend"`, and `"afterend"`.

## 4. Reading and Updating Content

| Property | Purpose |
|---|---|
| `element.textContent` | Get or set text content, including text in descendants |
| `element.innerText` | Get or set rendered text; affected by layout and CSS |
| `element.innerHTML` | Get or set the HTML markup inside an element |
| `element.outerHTML` | Get or set the element's HTML representation |

```js
const message = document.querySelector(".message");

message.textContent = "Saved successfully";
```

**Security:** Do not put untrusted user input into `innerHTML`. It can create cross-site scripting (XSS) vulnerabilities. Use `textContent` for plain text. If HTML is genuinely required, use a well-maintained sanitizer and apply an appropriate security policy.

## 5. Attributes and Classes

### Attributes

| Method / Property | Purpose |
|---|---|
| `element.getAttribute(name)` | Read an attribute |
| `element.setAttribute(name, value)` | Set an attribute |
| `element.removeAttribute(name)` | Remove an attribute |
| `element.hasAttribute(name)` | Check whether an attribute exists |
| `element.toggleAttribute(name)` | Toggle an attribute |
| `element.attributes` | Access the element's attribute collection |
| `element.dataset` | Read and write `data-*` attributes |
| `element.id` | Read or set the ID |
| `element.className` | Read or set the class string |

```html
<button id="save" data-state="idle">Save</button>
```

```js
const saveButton = document.querySelector("#save");

console.log(saveButton.dataset.state); // "idle"
saveButton.dataset.state = "saved";
saveButton.setAttribute("aria-pressed", "true");
```

### `classList`

| Method | Purpose |
|---|---|
| `element.classList.add(name)` | Add a class |
| `element.classList.remove(name)` | Remove a class |
| `element.classList.toggle(name)` | Toggle a class |
| `element.classList.contains(name)` | Check whether a class exists |
| `element.classList.replace(oldName, newName)` | Replace one class with another |

```js
saveButton.classList.add("is-active");
saveButton.classList.toggle("is-loading");
```

## 6. Event Handling

| Method / Property | Purpose |
|---|---|
| `element.addEventListener(type, handler, options)` | Register an event listener |
| `element.removeEventListener(type, handler, options)` | Remove a matching listener |
| `element.dispatchEvent(event)` | Dispatch an event |
| `event.preventDefault()` | Prevent the browser's default action |
| `event.stopPropagation()` | Stop propagation to other nodes |
| `event.stopImmediatePropagation()` | Stop propagation and later listeners on the same target |
| `event.target` | The original event target |
| `event.currentTarget` | The element whose listener is currently executing |
| `event.type` | The event type |
| `event.key` | The keyboard key associated with a keyboard event |
| `event.bubbles` | Whether the event bubbles |

```js
const list = document.querySelector("#list");

list.addEventListener("click", (event) => {
  const button = event.target.closest("button[data-action='delete']");

  if (!button || !list.contains(button)) return;

  button.closest("li")?.remove();
});
```

This demonstrates **event delegation**: one listener on a parent handles clicks from matching descendants.

### Event concepts to know

- **Bubbling:** an event propagates from the target up through ancestors.
- **Capturing:** an event travels from ancestors toward the target before the bubbling phase.
- **Delegation:** handle descendant events with a listener on a shared ancestor.
- **`target` vs `currentTarget`:** the original event target versus the element running the listener.
- **Cleanup:** use the same listener function when calling `removeEventListener()`.

## 7. Styles, Dimensions, and Scrolling

| Property / Method | Purpose |
|---|---|
| `element.style` | Read or set inline styles |
| `window.getComputedStyle(element)` | Read computed CSS styles |
| `element.getBoundingClientRect()` | Get viewport-relative position and dimensions |
| `element.offsetWidth` / `offsetHeight` | Layout size including padding and borders |
| `element.clientWidth` / `clientHeight` | Inner size including padding but excluding borders and scrollbars |
| `element.scrollWidth` / `scrollHeight` | Total content dimensions, including overflow |
| `element.scrollTop` / `scrollLeft` | Read or set scroll offsets |
| `element.scrollIntoView(options)` | Scroll an ancestor or viewport to reveal an element |
| `window.scrollTo(options)` | Scroll the document viewport to a position |
| `window.scrollBy(x, y)` | Scroll the viewport by an offset |

```js
const panel = document.querySelector(".panel");

console.log(panel.getBoundingClientRect());
console.log(getComputedStyle(panel).display);

panel.scrollIntoView({ behavior: "smooth", block: "nearest" });
```

## 8. Forms and Inputs

| Property / Method | Purpose |
|---|---|
| `input.value` | Read or set an input's value |
| `input.checked` | Read or set checkbox/radio state |
| `element.disabled` | Read or set the disabled state |
| `element.focus()` | Give an element keyboard focus |
| `element.blur()` | Remove focus |
| `input.select()` | Select the text in a supported text input |
| `form.reset()` | Reset form controls to their default values |
| `form.requestSubmit()` | Submit through normal submit-button behavior and validation |
| `form.checkValidity()` | Check built-in form validity |
| `form.reportValidity()` | Check validity and display validation feedback |
| `input.setCustomValidity(message)` | Set a custom validation message |
| `input.validity` | Inspect the input's validity state |
| `new FormData(form)` | Collect successful form controls into form data |

```js
const form = document.querySelector("#signup");

form.addEventListener("submit", (event) => {
  event.preventDefault();

  const data = new FormData(form);
  console.log(data.get("email"));
});
```

## 9. Document and Window APIs

| Property / Method | Purpose |
|---|---|
| `document.title` | Read or set the page title |
| `document.body` | Access the body element |
| `document.head` | Access the head element |
| `document.documentElement` | Access the root `<html>` element |
| `document.activeElement` | Get the currently focused element |
| `document.createDocumentFragment()` | Create a document fragment |
| `document.elementFromPoint(x, y)` | Find the topmost element at viewport coordinates |
| `window.getSelection()` | Access the current text selection |
| `window.matchMedia(query)` | Test a media query |
| `window.requestAnimationFrame(callback)` | Schedule a callback before a repaint |
| `window.cancelAnimationFrame(id)` | Cancel a scheduled animation frame |
| `window.requestIdleCallback(callback)` | Schedule work during idle time where supported |

## 10. Collections and Node Utilities

| Property / Method | Purpose |
|---|---|
| `NodeList.forEach(callback)` | Iterate over a `NodeList` |
| `Array.from(collection)` | Convert an iterable or array-like value to an array |
| `collection.length` | Get the number of entries |
| `element.isConnected` | Check whether an element is connected to a document |
| `node.compareDocumentPosition(otherNode)` | Compare positions of two nodes |
| `node.getRootNode()` | Get the node's root |

### `NodeList` vs `HTMLCollection`

| Feature | `NodeList` | `HTMLCollection` |
|---|---|---|
| Common source | `querySelectorAll()` | `getElementsByClassName()` |
| Contents | Depends on API; may contain nodes | Elements only |
| `forEach()` | Available on modern `NodeList` | Not generally available |
| Live or static | Depends on API; `querySelectorAll()` is static | Usually live |

A **static** collection does not automatically update after DOM changes. A **live** collection reflects applicable changes to the document.

## 11. Observers and Advanced APIs

| API | Purpose |
|---|---|
| `MutationObserver` | Observe DOM tree or attribute changes |
| `IntersectionObserver` | Detect intersection with a viewport or container |
| `ResizeObserver` | Observe element size changes |
| `AbortController` | Cancel supported operations using an abort signal |
| `addEventListener(type, handler, { signal })` | Tie event-listener cleanup to an abort signal |
| `requestAnimationFrame()` | Coordinate visual updates with browser rendering |
| `cancelAnimationFrame()` | Cancel a scheduled animation-frame callback |

Typical use cases:

- `MutationObserver`: observe changes made by code or third-party widgets.
- `IntersectionObserver`: lazy loading, infinite scrolling, visibility tracking.
- `ResizeObserver`: responsive charts and components whose layout depends on their container.
- `AbortController`: cancel fetch requests or clean up groups of event listeners.

## 12. Important Differences

### `parentElement` vs `parentNode`

- `parentElement` returns the parent only if it is an element.
- `parentNode` returns the parent node, which may be a document or document fragment.

### `children` vs `childNodes`

- `children` contains direct child elements only.
- `childNodes` includes elements, text nodes, and comments.

### `nextElementSibling` vs `nextSibling`

- `nextElementSibling` skips text and comment nodes.
- `nextSibling` returns the next node, which may be whitespace text.

### `querySelector()` vs `querySelectorAll()`

- `querySelector()` returns the first matching element or `null`.
- `querySelectorAll()` returns a static `NodeList` of matching elements.

### `textContent` vs `innerText` vs `innerHTML`

- `textContent`: text from descendants; does not parse assigned strings as HTML.
- `innerText`: rendered text; may depend on layout and CSS.
- `innerHTML`: parses or serializes markup; use caution with untrusted input.

### `event.target` vs `event.currentTarget`

- `target`: where the event originated.
- `currentTarget`: the element whose listener is running.

### `preventDefault()` vs `stopPropagation()`

- `preventDefault()` prevents a default browser action when the event is cancelable.
- `stopPropagation()` stops the event from continuing through the propagation path.
- They solve different problems and can be used independently.

## 13. Practice Examples

### Example 1: Find the nearest card

```js
document.addEventListener("click", (event) => {
  const card = event.target.closest(".card");
  if (card) console.log(card);
});
```

### Example 2: Add a list item safely

```js
const list = document.querySelector("#items");
const item = document.createElement("li");

item.textContent = "Learn DOM traversal";
list.append(item);
```

### Example 3: Toggle a class

```js
const menuButton = document.querySelector("#menu-button");
const menu = document.querySelector("#menu");

menuButton.addEventListener("click", () => {
  const isOpen = menu.classList.toggle("is-open");
  menuButton.setAttribute("aria-expanded", String(isOpen));
});
```

### Example 4: Observe when an element enters the viewport

```js
const target = document.querySelector(".lazy-section");

const observer = new IntersectionObserver((entries) => {
  for (const entry of entries) {
    if (entry.isIntersecting) {
      console.log("Section is visible");
      observer.unobserve(entry.target);
    }
  }
});

observer.observe(target);
```

### Example 5: Clean up event listeners with `AbortController`

```js
const controller = new AbortController();

window.addEventListener(
  "resize",
  () => console.log("Window resized"),
  { signal: controller.signal }
);

// Later, remove listeners registered with this signal.
controller.abort();
```

## 14. Interview Preparation Checklist

Use this checklist while studying:

### Selection and traversal
- [ ] `querySelector()` and `querySelectorAll()`
- [ ] `getElementById()`
- [ ] `closest()` and `matches()`
- [ ] `parentElement`, `children`, and `childNodes`
- [ ] `firstElementChild` and `lastElementChild`
- [ ] `nextElementSibling` and `previousElementSibling`
- [ ] `contains()` and `hasChildNodes()`

### Manipulation and content
- [ ] `createElement()` and `createTextNode()`
- [ ] `append()`, `prepend()`, and `appendChild()`
- [ ] `before()`, `after()`, and `insertBefore()`
- [ ] `remove()`, `replaceWith()`, and `cloneNode()`
- [ ] `textContent` vs `innerText` vs `innerHTML`
- [ ] Attributes, `dataset`, and `classList`

### Events
- [ ] `addEventListener()` and `removeEventListener()`
- [ ] `event.target` vs `event.currentTarget`
- [ ] `preventDefault()` vs `stopPropagation()`
- [ ] Bubbling and capturing
- [ ] Event delegation
- [ ] Keyboard, mouse, and form events

### Browser fundamentals
- [ ] `getComputedStyle()` and `element.style`
- [ ] `getBoundingClientRect()`
- [ ] Focus and form validation
- [ ] `NodeList` vs `HTMLCollection`
- [ ] Live vs static collections
- [ ] `MutationObserver` and `IntersectionObserver`

---

## React Developer Note

In React applications, prefer React state and props for rendering UI. Use refs for focused imperative tasks such as focusing an input, measuring an element, or integrating with a browser API. Avoid manually changing DOM nodes that React owns unless there is a specific reason.

## Further Reading

- [MDN: Document Object Model (DOM)](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)
- [MDN: Element](https://developer.mozilla.org/en-US/docs/Web/API/Element)
- [MDN: Node](https://developer.mozilla.org/en-US/docs/Web/API/Node)
- [MDN: Event](https://developer.mozilla.org/en-US/docs/Web/API/Event)
- [MDN: Introduction to events](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Scripting/Events)

---

**Goal:** Learn what each API returns, whether it works with elements or all nodes, and when it is appropriate to use it. Practice the core methods in small HTML examples rather than memorizing names alone.
