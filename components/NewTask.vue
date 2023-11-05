
<script setup lang="ts">
import type { Task } from "./types/index.ts"
import { nanoid } from 'nanoid';
const emit = defineEmits(["add"])
const focused = ref(false)
const title = ref("")
function createTask(e: Event) {
    console.log(e)
    if (title.value != "") {
        e.preventDefault();
        emit("add", {
            title: title.value.trim(),
            createdAt: new Date(),
            id: nanoid(),
        } as Task);
    }

    title.value = "";

}

</script>
<template>
    <div>
        <textarea v-model="title" @keydown.tab="createTask" @keyup.enter="createTask"
            class="focus:bg-white focus:shadow resize-none rounded w-full border-none" style="outline:none !important"
            @focus="focused = true" @blur="focused = false"
            :placeholder="!focused ? '+ Add A Card' : 'Enter a title for this card'">

    </textarea>
    </div>
</template>