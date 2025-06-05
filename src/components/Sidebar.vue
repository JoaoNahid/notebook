<script setup>
const props = defineProps({'active-note': String, 'notes': Array});
</script>

<template>
    <aside class="w-[256px] min-h-screen bg-gray-800 text-white flex flex-col">
        <div class="p-4">
            <button class="button w-full mb-2" @click="$emit('create-note')">
                New Note
            </button>

            <ul>
                <li 
                    v-for="note in props.notes"
                    :key="note.id"
                    class="note-list-item"
                    @click="$emit('set-active-note', note.id)"
                >
                    <span>{{ note.title }}</span>
                    <button
                        @click.prevent="$emit('delete-note', note.id)"
                        class="text-red-500 cursor-pointer"
                    >
                        Delete
                    </button>
                </li>
            </ul>
        </div>
    </aside>
</template>