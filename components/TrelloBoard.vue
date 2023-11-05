<script setup lang="ts">
import type { Column, Task } from '../types/index.ts';
import { nanoid } from "nanoid";
import TrelloBoardTask from './TrelloBoardTask.vue';
import draggable from 'vuedraggable';
const columns = ref<Column[]>([
    {
        id: nanoid(),
        title: "Backlog",
        tasks: [
            {
                id: nanoid(),
                title: "Create marketing landing page",
                createdAt: new Date(),
            },
            {
                id: nanoid(),
                title: "Develop cool new feature",
                createdAt: new Date(),
            },
            {
                id: nanoid(),
                title: "Fix page nav bug",
                createdAt: new Date(),
            },
        ]
    },
    { id: nanoid(), title: "Selected for Dev", tasks: [] },
    { id: nanoid(), title: "In Progress", tasks: [] },
    { id: nanoid(), title: "QA", tasks: [] },
    { id: nanoid(), title: "Complete", tasks: [] },
])

</script>
<template>
    <div class="flex gap-4 overflow-x-auto items-start">
        <draggable 
        v-model="columns" 
        group="columns" 
        :animation="150"
        handle=".drag-handle"
        item-key="id"
        class="flex gap-4 overflow-x-auto items-start"
        >
        <template #item="{element: column} : {element: Column}">
            <div  class="column bg-gray-200 p-5 rounded min-w-[250px]">
            <header class="font-bold mb-2">
              <DragHandle />
                {{ column.title }}
            </header>
            <draggable 
        v-model="column.tasks" 
        group="tasks" 
        handle=".drag-handle"
        :animation="150"
        item-key="id"
        >
        <template #item="{element: task} : {element: Task}">
            <TrelloBoardTask :task="task" />
        </template>
            </draggable>
          
            <footer>
                <button class="text-gray-500">+ Add a Card</button>
            </footer>
        </div>
        </template>
        </draggable>
    </div>
</template>
