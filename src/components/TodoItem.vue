<script setup lang="ts">
import type { Todo } from "../models/todo.ts";


interface Props {
  todo: Todo;
}

const props = defineProps<Props>();

const emit = defineEmits<{
    (e: "toggle", id: string): void;
    (e: "delete", id: string): void;
}>();
</script>

<template>
    <li>
        <input type="checkbox" :checked="todo.done" @change="emit('toggle', todo.id)" />
        <span :style="{ textDecoration: todo.done ? 'line-through' : 'none' }">
            {{ todo.text }}
        </span>
        <button @click="emit('delete', todo.id)">Löschen</button>
    </li>
</template>