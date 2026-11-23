<script setup>
import { priorities } from '../priorities.js';

defineProps(['items', 'title']);
</script>
<template>
    <h1>{{ title }}</h1>
    <ul>
        <li v-for="item in items" :key="item.id" :class="'has-text-' + priorities[item.priority].color">
            {{ item.text }}
            <span class="tag" :class="'is-' + priorities[item.priority].color">{{ priorities[item.priority].label }}</span>
            <input type="checkbox" v-model="item.isDone">
        </li>
    </ul>
</template>
