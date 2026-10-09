<script setup>
import { ref } from 'vue'

const emit = defineEmits(['add'])

const title = ref('')
const author = ref('')
const genre = ref('Fiction')
const status = ref('Want to Read')
const error = ref('')

function submit() {
  if (title.value.trim() === '' || author.value.trim() === '') {
    error.value = 'Title and author are required.'
    return
  }

  emit('add', {
    title: title.value.trim(),
    author: author.value.trim(),
    genre: genre.value,
    status: status.value
  })

  title.value = ''
  author.value = ''
  genre.value = 'Fiction'
  status.value = 'Want to Read'
  error.value = ''
}
</script>

<template>
  <div class="book-form">
    <h2>Add a Book</h2>

    <input v-model="title" type="text" placeholder="Title" @keyup.enter="submit" />
    <input v-model="author" type="text" placeholder="Author" @keyup.enter="submit" />

    <div class="row">
      <select v-model="genre">
        <option>Fiction</option>
        <option>Non-Fiction</option>
        <option>Science</option>
        <option>Technology</option>
        <option>History</option>
        <option>Other</option>
      </select>

      <select v-model="status">
        <option>Want to Read</option>
        <option>Reading</option>
        <option>Finished</option>
      </select>

      <button class="primary" @click="submit">Add</button>
    </div>

    <p v-if="error" class="error">{{ error }}</p>
  </div>
</template>

<style scoped>
.book-form {
  display: flex;
  flex-direction: column;
  gap: 10px;
  background: #fff;
  padding: 16px;
  border-radius: 8px;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.1);
}

h2 {
  margin: 0;
  font-size: 18px;
}

.row {
  display: flex;
  gap: 10px;
}

.row select {
  flex: 1;
}

.primary {
  background: #2f6fed;
  color: #fff;
}

.error {
  margin: 0;
  color: #d93025;
  font-size: 14px;
}
</style>
