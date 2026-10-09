<script setup>
import { ref } from 'vue'

const props = defineProps({
  book: Object
})

const emit = defineEmits(['update', 'delete'])

const isEditing = ref(false)
const editedTitle = ref(props.book.title)
const editedAuthor = ref(props.book.author)
const editedGenre = ref(props.book.genre)
const editedStatus = ref(props.book.status)

function startEditing() {
  editedTitle.value = props.book.title
  editedAuthor.value = props.book.author
  editedGenre.value = props.book.genre
  editedStatus.value = props.book.status
  isEditing.value = true
}

function saveEdit() {
  if (editedTitle.value.trim() === '' || editedAuthor.value.trim() === '') {
    return
  }

  emit('update', {
    ...props.book,
    title: editedTitle.value.trim(),
    author: editedAuthor.value.trim(),
    genre: editedGenre.value,
    status: editedStatus.value
  })

  isEditing.value = false
}

function deleteBook() {
  if (confirm(`Delete "${props.book.title}"?`)) {
    emit('delete', props.book.id)
  }
}
</script>

<template>
  <div class="book-item">
    <!-- Normal mode -->
    <div v-if="!isEditing" class="view">
      <div class="info">
        <strong>{{ book.title }}</strong>
        <span class="author">by {{ book.author }}</span>
        <div>
          <span class="tag">{{ book.genre }}</span>
          <span class="tag" :class="book.status.replace(/\s/g, '').toLowerCase()">
            {{ book.status }}
          </span>
        </div>
      </div>

      <div class="actions">
        <button class="edit" @click="startEditing">Edit</button>
        <button class="delete" @click="deleteBook">Delete</button>
      </div>
    </div>

    <!-- Edit mode -->
    <div v-else class="editing">
      <input v-model="editedTitle" type="text" placeholder="Title" />
      <input v-model="editedAuthor" type="text" placeholder="Author" />

      <div class="row">
        <select v-model="editedGenre">
          <option>Fiction</option>
          <option>Non-Fiction</option>
          <option>Science</option>
          <option>Technology</option>
          <option>History</option>
          <option>Other</option>
        </select>

        <select v-model="editedStatus">
          <option>Want to Read</option>
          <option>Reading</option>
          <option>Finished</option>
        </select>
      </div>

      <div class="actions">
        <button class="save" @click="saveEdit">Save</button>
        <button class="cancel" @click="isEditing = false">Cancel</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.book-item {
  background: #fff;
  padding: 14px;
  border-radius: 8px;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.1);
}

.view {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 12px;
}

.info {
  display: flex;
  flex-direction: column;
  gap: 4px;
}

.author {
  color: #666;
  font-size: 14px;
}

.tag {
  display: inline-block;
  margin: 2px 6px 0 0;
  padding: 2px 8px;
  border-radius: 12px;
  background: #e8eaed;
  font-size: 12px;
}

.tag.wanttoread {
  background: #fff3cd;
}

.tag.reading {
  background: #d6e6ff;
}

.tag.finished {
  background: #d4f1dc;
}

.actions {
  display: flex;
  gap: 6px;
}

.editing {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.row {
  display: flex;
  gap: 10px;
}

.row select {
  flex: 1;
}

.edit,
.save {
  background: #2f6fed;
  color: #fff;
}

.delete {
  background: #d93025;
  color: #fff;
}

.cancel {
  background: #e0e0e0;
}
</style>
