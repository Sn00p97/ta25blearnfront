<script setup>
import { computed, ref } from 'vue';
import ItemList from '../components/ItemList.vue';
import { priorities } from '../priorities.js';

let message = ref('');
let priority = ref('medium');
let i = 1;
let items = ref([
    { id: i++, text: 'Piim', isDone: false, priority: 'high' },
    { id: i++, text: 'Viin', isDone: true, priority: 'low' },
    { id: i++, text: 'Õlu', isDone: false, priority: 'medium' },
    { id: i++, text: 'Sai', isDone: true, priority: 'high' },
    { id: i++, text: 'Kommid', isDone: false, priority: 'low' },
]);

function add() {
    if (message.value.trim() !== '') {
        items.value.push({ id: i++, text: message.value.trim(), isDone: false, priority: priority.value });
    }
    message.value = '';
}

// kõige olulisem ees; koopia ([...]), et algset massiivi mitte muuta
let sortedItems = computed(() =>
    [...items.value].sort((a, b) => priorities[a.priority].order - priorities[b.priority].order)
);
let doneItems = computed(() => sortedItems.value.filter(item => item.isDone));
let toDoItems = computed(() => sortedItems.value.filter(item => !item.isDone));
</script>

<template>

    <div class="field has-addons">
        <div class="control is-expanded">
            <input @keydown.enter="add" v-model="message" class="input" type="text" placeholder="Add item">
        </div>
        <div class="control">
            <div class="select">
                <select v-model="priority">
                    <option v-for="(p, key) in priorities" :key="key" :value="key">{{ p.label }}</option>
                </select>
            </div>
        </div>
        <div class="control">
            <button @click="add" class="button is-info">
                Add
            </button>
        </div>
    </div>

    <ItemList :items="sortedItems" title="All items"></ItemList>
    <ItemList :items="doneItems" title="Done items"></ItemList>
    <ItemList :items="toDoItems" title="ToDo items"></ItemList>
</template>

<style></style>
