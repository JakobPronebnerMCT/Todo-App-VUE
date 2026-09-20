<script setup lang="ts">
import { ref, computed } from "vue";
import type { Todo } from "../models/todo.ts";
import TodoList from "./TodoList.vue";

type FilterStatus = "all" | "open" | "done";

const todos = ref<Todo[]>([]);
const newText = ref("");

const currentFilter = ref<FilterStatus>("all");

function addTodo(): void {
    if (newText.value.trim() === "") return;

    todos.value.push({
        id: crypto.randomUUID(),
        text: newText.value.trim(),
        done: false,
    });

    newText.value = "";
}

function handleToggle(id: string): void {
    const todo = todos.value.find(t => t.id === id);
    if (todo) {
        todo.done = !todo.done;
    }
}

function handleDelete(id: string): void {
    todos.value = todos.value.filter(t => t.id !== id);
}

const filteredTodos = computed(() => {
    if (currentFilter.value === "open") {
        return todos.value.filter(t => !t.done);
    }
    if (currentFilter.value === "done") {
        return todos.value.filter(t => t.done);
    }
    return todos.value;
});
</script>

<template>
    <div>
        <input v-model="newText" placeholder="Neues Todo:" />
        <button @click="addTodo">Hinzufügen</button>

        <div>
            <button @click="currentFilter = 'all'">Alle</button>
            <button @click="currentFilter = 'open'">Offen</button>
            <button @click="currentFilter = 'done'">Erledigt</button>
        </div>

        <TodoList 
            :todos="filteredTodos" 
            @toggle="handleToggle" 
            @delete="handleDelete" 
        />
    </div>
</template>