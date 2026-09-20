<script setup lang="ts">
import type { Todo } from "../models/todo.ts";
import TodoItem from "./TodoItem.vue";

interface Props {
  todos: Todo[];
}

const props = defineProps<Props>();

const emit = defineEmits<{
    (e: "toggle", id: string): void;
    (e: "delete", id: string): void;
}>();

</script>

<template>
    <ul>
        <TodoItem 
            v-for="item in todos" 
            :key="item.id" 
            :todo="item" 
            @toggle="emit('toggle', $event)" 
            @delete="emit('delete', $event)" 
        />
    </ul>
</template>