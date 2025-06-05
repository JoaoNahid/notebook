<script setup>
import { ref } from 'vue';
import Sidebar from './components/Sidebar.vue';

const notes = ref([])

const activeNote = ref(null);

const inputTitle = ref('');
const inputContent = ref('');

// Create note
function createNote () {
    // tandom id
    const id = Math.random().toString(36).substring(2, 9);
    notes.value.push({
        id,
        title: 'Title',
        content: '',
    })

    setActiveNote(id);
}

// Set active note
function setActiveNote(id) {
    activeNote.value = id;
    const note = notes.value.find(note => note.id === id);

    inputTitle.value = note.title;
    inputContent.value = note.content;

    setTimeout(() => {
        document.querySelector('input[type="text"]').focus();
    }, 0);
}

// Delete note
function deleteNote(id) {
    let noteId = notes.value.findIndex(note => note.id === id);
    notes.value.splice(noteId, 1);

    activeNote.value = null;
    inputTitle.value = '';
    inputContent.value = '';
}

// Update note
function updateNote() {
    let note = notes.value.findIndex(note => note.id === activeNote.value);

    notes.value[note].title = inputTitle.value;
    notes.value[note].content = inputContent.value;
}
</script>

<template>
    <div class="bg-gray-700 text-white min-h-screen flex items-stretch justify-start">
        <!-- Sidebar -->
        <Sidebar
            :active-note="activeNote"
            :notes="notes"
            @create-note="createNote"
            @set-active-note="setActiveNote"
            @delete-note="deleteNote"
        />

        <!-- Main Content -->
        <main class="flex-1">
            <div v-if="activeNote" class="px-4 py-8 flex flex-col h-full">
                <input
                    v-model="inputTitle"
                    @input="updateNote"
                    type="text"
                    name="title"
                    placeholder="Title"
                    class="block w-full text-3xl pg-2 font-bold border-b-2 border-gray-500 focus:border-white outline-none transition-colors duration-200"
                />
    
                <textarea
                    v-model="inputContent"
                    @input="updateNote"
                    name="content"
                    placeholder="Write your note here..."
                    class="block w-full h-full mt-4 text-lg outline-none flex-1"
                ></textarea>
            </div>
        </main>
    </div>
</template>

