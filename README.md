# Expense Tracker

A clean, focused expense tracking app built with React 19 and Tailwind CSS 4. Add an expense with a title and amount, delete it with a single click, and see the running total update instantly — all persisted to `localStorage` so nothing is lost on a page refresh.

---

## What This Project Does

The app renders a single card-style interface centered on screen. An `AddExpenses` form sits at the top, and `AllExpenses` renders the list below it with a live total. State is initialized directly from `localStorage` in `useState`'s initializer function, so previous data loads instantly on first render without an extra effect. Any mutation to the `Expenses` array triggers a `useEffect` that writes the updated data back to `localStorage`. Everything is a single page — no routing, no backend, no authentication.

---

## Features

**1. Add expenses** — The form accepts two fields: a capitalized title (text input) and an amount (number input). Both fields are controlled components. Clicking "Add" validates that neither field is empty before calling `onAddExpense`, appending a new entry with a `Date.now()` id to the front of the list, then clearing both inputs ready for the next entry.

**2. Delete expenses** — Every expense row in `AllExpenses` has a red "Delete" button. Clicking it calls `onDeleteExpense` with the entry's id, which filters that item from the `Expenses` array. The UI and `localStorage` update simultaneously.

**3. LocalStorage persistence** — The `Expenses` state initializer reads from `localStorage` on mount. A `useEffect` that depends on `[Expenses]` writes back to `localStorage` on every change. Data survives full page reloads without any server call.

**4. Live total calculation** — `AllExpenses` uses `Array.reduce` to sum all `expense.amount` values and displays the result formatted to 2 decimal places. It recalculates on every render driven by the current `expenses` prop.

**5. Empty state** — When no expenses have been added, `AllExpenses` renders a centered "No expenses added yet." message instead of an empty list, keeping the UI informative at all times.

**6. Fully responsive layout** — The form inputs and expense rows both use `flex-col sm:flex-row` to stack vertically on mobile and sit side-by-side on wider screens. All spacing uses Tailwind utility classes — no custom CSS or breakpoints.

---

## Tech Stack

| Technology | Version | Role |
|---|---|---|
| React | 19.2.0 | Component architecture, controlled inputs, state management, JSX rendering |
| Tailwind CSS | 4.1.17 | All utility-class styling — layout, spacing, colors, responsive breakpoints |
| React Icons | 5.5.0 | UI icons used in the interface |
| Vite | 7.2.4 | Build tool and dev server (uses `@tailwindcss/vite` plugin — no separate PostCSS config needed) |
| ESLint | 9.39.1 | Linting with `eslint-plugin-react-hooks` and `eslint-plugin-react-refresh` |
| LocalStorage API | — | Client-side data persistence — read on mount, written on every state change |

---

## State Management

The app uses React `useState` and `useEffect` exclusively — no external state library.

| State variable | Type | Location | Purpose |
|---|---|---|---|
| `Expenses` | `array` | `App.jsx` | List of all expense objects `{ id, title, amount }` — initialized from `localStorage` |
| `title` | `string` | `AddExpenses.jsx` | Controlled input for the expense title field |
| `amount` | `string` | `AddExpenses.jsx` | Controlled input for the expense amount field |

`App.jsx` owns `Expenses` and passes `onAddExpense` and `onDeleteExpense` as props down to the two child components. No prop drilling beyond one level — there is no Context API or global store.

---

## Project Structure

```
Expense-Tracker/
├── index.html                      # Vite entry point — single div#root mount target
├── package.json                    # Dependencies: react 19, @tailwindcss/vite 4, react-icons, vite 7
├── package-lock.json               # Exact dependency lockfile
├── vite.config.js                  # Vite configuration — React plugin + @tailwindcss/vite plugin (replaces postcss setup)
├── eslint.config.js                # ESLint flat config — react-hooks and react-refresh rules
├── .gitignore                      # node_modules, dist excluded
└── src/
    ├── main.jsx                    # React DOM root render — mounts <App /> into #root
    ├── index.css                   # Tailwind CSS import directive
    ├── App.jsx                     # Root component — owns Expenses state, handles localStorage sync via useEffect, defines AddExpense and DeleteExpense handlers, renders AddExpenses + AllExpenses inside a centered card
    └── components/
        ├── AddExpenses.jsx         # Controlled form — title (text) and amount (number) inputs with local useState; calls onAddExpense prop on submit if both fields are non-empty, then clears both inputs
        └── AllExpenses.jsx         # Expense list — maps expenses array into rows; each row shows capitalized title, formatted $amount, and a Delete button; renders total via reduce at the top; shows empty state when array is empty
```

---

## How to Run

1. Clone the repository
   ```bash
   git clone https://github.com/tripathipawan/Expense-Tracker.git
   ```

2. Move into the project directory
   ```bash
   cd Expense-Tracker
   ```

3. Install dependencies
   ```bash
   npm install
   ```

4. Start the development server
   ```bash
   npm run dev
   ```

5. Open `http://localhost:5173`. Add a few expenses using the form — they will appear in the list immediately with the running total, and will still be there after a page refresh.

6. To build for production:
   ```bash
   npm run build
   ```
   Output goes into `dist/`, ready to deploy to any static hosting service.

---

## Repository

[https://github.com/tripathipawan/Expense-Tracker](https://github.com/tripathipawan/Expense-Tracker)
