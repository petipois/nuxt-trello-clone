
<script setup lang="ts">
import type { Task, Column } from "./types/index.ts"
import { ref } from "vue";
import { nanoid } from "nanoid"
import draggable from "vuedraggable"
const columns = useLocalStorage<Column[]>("gameFlowBoard", [
    {
        id: nanoid(),
        title: "Landing page",
        tasks: [{
            id: nanoid(),
            title: "Create game logo",
            createdAt: new Date(),
        },
        {
            id: nanoid(),
            title: "Prototype webpage with figma",
            createdAt: new Date(),
        }]
    },
    {
        id: nanoid(),
        title: "Game Jam Day 1",
        tasks: []
    },
    {
        id: nanoid(),
        title: "Game Jam Day 2",
        tasks: []
    },
    {
        id: nanoid(),
        title: "Game Jam Day 3",
        tasks: []
    },

])
const alt = useKeyModifier("Alt")
const createColumn = () => {
    const column: Column = {
        id: nanoid(),
        title: "",
        tasks: []
    }
    columns.value.push(column)
}
watch(columns,()=>{
//ajax request
},{
    deep:true
})
</script>

<template>
    <div class="grid items-start overflow-x-auto">
        <draggable v-model="columns" :animation="150" handle=".drag-handle" group="columns" item-key="id"
            class="grid grid-cols-2">
            <template #item="{ element: column }: { element: Column }">
                <div class="column bg-blue-200 p-5 min-w-[250px] rounded m-2">
                    <header class="font-bold mb-4">
                        <DragHandle />
                        <input class="bg-transparent focus:bg-white rounded px-1 w-4/5"
                            @keydown.backspace="column.title === '' ? columns = columns.filter(c => c.id != column.id) : null"
                            @keyup.enter="($event.target as HTMLInputElement).blur()" type="text" v-model="column.title" />
                    </header>
                    <draggable v-model="column.tasks" :group="{ name: 'tasks', pull: alt ? 'clone' : true }"
                        :animation="150" handle=".drag-handle" item-key="id">
                        <template #item="{ element: task }: { element: Task }">
                            <div>
                                <BoardTask :task="task"
                                    @delete="column.tasks = column.tasks.filter((t) => t.id != $event)" />
                            </div>

                        </template>
                    </draggable>
                    <footer>
                        <NewTask @add="column.tasks.push($event)" />
                    </footer>
                </div>
            </template>
        </draggable>
        <button @click="createColumn" class="bg-gray-200 p-2 rounded opacity-50">+ Add Another
            Column</button>
    </div>
</template>