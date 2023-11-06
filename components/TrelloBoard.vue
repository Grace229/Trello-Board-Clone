<script setup lang="ts">
import type { Column, Task } from '../types/index.ts';
import { nanoid } from "nanoid";
import TrelloBoardTask from './TrelloBoardTask.vue';
import draggable from 'vuedraggable';
import NewTask from './NewTask.vue';
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
]);
const alt = useKeyModifier("Alt")

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
        :group="{name: 'tasks', pull: alt ?  'clone' : true}" 
        handle=".drag-handle"
        :animation="150"
        item-key="id"
        >
        <template #item="{element: task} : {element: Task}">
            
            <div>
                <TrelloBoardTask :task="task" />
            </div>
        </template>
            </draggable>
          
            <footer>
                <NewTask @add="column.tasks.push($event)"  />
            </footer>
        </div>
        </template>
        </draggable>
    </div>
</template>
<style>
/* draggable class added to element when you first click on it to drag it */
.sortable-chosen{

}
/* applies when you start moving the item */
.sortable-drag .task{
transform: rotate(5deg);
}
/* target element that are beneath the card */
.sortable-ghost .task{
position: relative;
}
.sortable-ghost .task::after{
content: "";
@apply absolute top-0 bottom-0 left-0 right-0 bg-slate-300 rounded
}
</style>