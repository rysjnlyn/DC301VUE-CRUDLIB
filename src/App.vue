<script setup>
import { ref, computed, onMounted } from 'vue'
import BookForm from './components/BookForm.vue'
import BookItem from './components/BookItem.vue'

const STORAGE_KEY = 'books'

const books = ref([])
const search = ref('')
const statusFilter = ref('All')

// ---------- CREATE ----------
function addBook(bookData) {
  books.value.push({
    id: Date.now(),
    ...bookData
  })
  saveBooks()
}

// ---------- UPDATE ----------
function updateBook(updatedBook) {
  const index = books.value.findIndex(book => book.id === updatedBook.id)

  if (index !== -1) {
    books.value[index] = updatedBook
  }
  saveBooks()
}

// ---------- DELETE ----------
function deleteBook(id) {
  books.value = books.value.filter(book => book.id !== id)
  saveBooks()
}

// ---------- READ (load + filter) ----------
function saveBooks() {
  localStorage.setItem(STORAGE_KEY, JSON.stringify(books.value))
}

onMounted(() => {
  const saved = localStorage.getItem(STORAGE_KEY)
  if (saved) {
    books.value = JSON.parse(saved)
  }
})

const filteredBooks = computed(() => {
  const term = search.value.trim().toLowerCase()

  return books.value.filter(book => {
    const matchesSearch =
      book.title.toLowerCase().includes(term) ||
      book.author.toLowerCase().includes(term)
    const matchesStatus =
      statusFilter.value === 'All' || book.status === statusFilter.value

    return matchesSearch && matchesStatus
  })
})

const readCount = computed(
  () => books.value.filter(book => book.status === 'Finished').length
)
</script>

<template>
  <div class="container">
    <h1>📚 My Book Library</h1>
    <p class="subtitle">
      {{ books.length }} book(s) · {{ readCount }} finished
    </p>

    <BookForm @add="addBook" />

    <div class="toolbar">
      <input v-model="search" type="text" placeholder="Search title or author" />
      <select v-model="statusFilter">
        <option>All</option>
        <option>Want to Read</option>
        <option>Reading</option>
        <option>Finished</option>
      </select>
    </div>

    <div class="book-list">
      <BookItem
        v-for="book in filteredBooks"
        :key="book.id"
        :book="book"
        @update="updateBook"
        @delete="deleteBook"
      />

      <p v-if="filteredBooks.length === 0" class="empty">
        No books found. Add one above!
      </p>
    </div>
  </div>
</template>

<style>
.container {
  max-width: 640px;
  margin: 40px auto;
  padding: 0 16px;
}

h1 {
  text-align: center;
  margin-bottom: 4px;
}

.subtitle {
  text-align: center;
  color: #666;
  margin-top: 0;
}

.toolbar {
  display: flex;
  gap: 10px;
  margin: 20px 0 14px;
}

.toolbar input {
  flex: 1;
}

.book-list {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.empty {
  text-align: center;
  color: #888;
}
</style>
