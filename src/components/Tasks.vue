<template>

    <section id="tasks">

        <FormTask 
            :form="form"
            @addTask="addTask"
            @updateTask="updateTask"
        />

        <div class="tasks-list">

            <div class="task-item">

                <draggable 
                    v-model="undoneTasks" 
                    @start="drag=true" 
                    @end="drag=false" 
                    item-key="id">
                    <template #item="{element}">
                        
                        <Task 
                            :task="element"
                            @remove="removeTask"
                            @done="doneTask"
                            @edit="editTask"
                        />

                    </template>
                </draggable>

            </div>

            <div class="task-item-active">
                
                <draggable 
                    v-model="donedTasks"
                    tag="transition-group"  
                    @start="drag=true" 
                    @end="drag=false" 
                    item-key="id">
                    <template #item="{element}">
                        
                        <Task 
                            :task="element"
                            @remove="removeTask"
                            @done="doneTask"
                            @edit="editTask"
                        />

                    </template>
                </draggable>

            </div>

            <p v-if="tasksCount < 1" class="empty-task">dont have any task</p>

        </div>

    </section>

</template>

<script>
import { computed, reactive, ref } from 'vue'

// components
import Task from '@/components/Task'
import FormTask from '@/components/FormTask'
import draggable from 'vuedraggable'

export default {
    components: {
        FormTask,
        Task,
        draggable
    },

    setup(){
        // data
        const tasks = ref([]) 
        const form = reactive({
            // only should have (edit or add)
            status: 'add'
        })
        const drag = ref(false)

        // computed
        const tasksCount = computed(() => {
            return tasks.value.length
        })

        const donedTasks = computed({
            get(){  
                // get only done tasks   
                const data = []
                
                tasks.value.map(item => {
                    if(item.done)
                        data.push(item)
                })

                return data
            },
            set(value){
                //  create a new tasks Retrieved from undone and doned
                const newTasks = [...undoneTasks.value, ...value]

                // set new tasks to the app
                tasks.value = newTasks

                // save new tasks in the localStorage
                localStorage.setItem('tasks', JSON.stringify(tasks.value))
            }
        })
        const undoneTasks = computed({
            get(){
                // get only undone tasks
                const data = []

                tasks.value.map(item => {
                    if(!item.done)
                        data.push(item)
                })

                return data
            },
            set(value){
                //  create a new tasks Retrieved from undone and doned
                const newTasks = [...value, ...donedTasks.value]

                // set new tasks to the app
                tasks.value = newTasks

                // save new tasks in the localStorage
                localStorage.setItem('tasks', JSON.stringify(tasks.value))
            }
        })

        // methods
        const addTask = (task) => {
            // create id
            const id = tasks.value.reduce((max, item) => (item.id > max) ? max = item.id : max, 0) + 1
            
            // create new task
            const data = {
                id,
                content: task,
                done: false
            }

            // add new task to the tasks
            tasks.value.push(data)

            // save tasks to the localStorage
            localStorage.setItem('tasks', JSON.stringify(tasks.value))
        }

        const getTasks = () => {
            // get tasks from localStorage
            const data = JSON.parse(localStorage.getItem('tasks'))

            // if be any task in the localStorage come set
            if(data)
                tasks.value = data
        }
        
        getTasks()

        const removeTask = (id) => {
            // find task from id
            const array = tasks.value

            array.map((item, index) => {
                // if finded task come delete task
                if(item.id == id)
                    tasks.value.splice(index, 1)
            })  

            // then set new tasks in the localStorage
            localStorage.setItem('tasks', JSON.stringify(tasks.value))
        }

        const doneTask = (id) => {
            // find task by base id
            const array = tasks.value

            array.map(item => {
                // if finded task come change status - done or undone 
                if(item.id == id)
                    item.done = !item.done
            })

            // then set new tasks in the localStorage
            localStorage.setItem('tasks', JSON.stringify(tasks.value))
        }

        const editTask = (id) => {
            // change status form to edit
            form.status = 'edit'

            // find task
            let task;

            tasks.value.map(item => {
                if(item.id == id)
                    task = item
            })

            // set task to form data for replace in update
            form.data = task        
        }

        const updateTask = (task) => {
            // find and update task
            tasks.value.map(item => {
                if(item.id == form.data.id)
                    item.content = task
            }) 

            // change status form to add
            form.status = 'add'

            // save new tasks to the localStorage
            localStorage.setItem('tasks', JSON.stringify(tasks.value))
        }

        return { 
            tasks, tasksCount, donedTasks,
            undoneTasks, form, drag,
            addTask, removeTask, doneTask, editTask, updateTask
        }
    }
}
</script>

<style scoped>

    .task-item-active{
        margin-top: 2.75rem;
    }

    .task-item-active .task{
        background: rgba(255, 255, 255, 0.25);
        text-decoration: line-through;
        color: #757575;
    }

    .empty-task{
        text-align: center;
        font-weight: 600;
        font-size: 0.9rem;
        color: #454545;
    }

</style>