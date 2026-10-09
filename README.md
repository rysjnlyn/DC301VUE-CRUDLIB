# Book Library – Vue 3 CRUD App

A simple book library built with Vue 3 + Vite for DC301.

## Features (CRUD)

- **Create** – add a book (title, author, genre, reading status)
- **Read** – list all books, search by title/author, filter by status
- **Update** – edit a book inline, with Save / Cancel
- **Delete** – remove a book (with confirmation)

Data is stored in the browser's `localStorage`, so it persists after refresh.

## Project structure

```
src/
├── App.vue                  # state + CRUD functions, search/filter
└── components/
    ├── BookForm.vue         # Create form
    └── BookItem.vue         # Display / edit / delete a single book
```

## Run it

```bash
npm install
npm run dev
```

Then open the local URL shown in the terminal (usually http://localhost:5173).
