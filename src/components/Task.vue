<template>

    <div class="task">

        <h3 class="task-content">{{task.content}}</h3>

        <div class="task-actions">

            <button @click="editTask(task.id)" class="task-btn task-edit-btn">
                <i class="fas fa-pen"></i>
            </button>

            <button @click="deleteTask(task.id)" class="task-btn task-delete-btn">
                <i class="fas fa-trash"></i>
            </button>

            <button @click="doneTask(task.id)" class="task-btn task-done-btn">
                
                <span v-if="task.done">
                    <i class="fas fa-check-circle"></i>
                </span>

                <span v-else>
                    <i class="far fa-check-circle"></i>
                </span>

            </button>

        </div>

    </div>

</template>

<script>
export default {
    props: ['task'],

    setup(props, { emit }){
        // methods
        const deleteTask = (id) => {
            emit('remove', id)
        }

        const doneTask = (id) => {
            emit('done', id)
        }

        const editTask = (id) => {
            emit('edit', id)
        }

        return { deleteTask, doneTask, editTask }
    }
}
</script>

<style scoped>

    .task{
        background: white;
        box-shadow: 0 0 15px 5px rgba(0, 0, 0, 0.11);
        margin: 1rem 0.5rem;
        padding: 1rem;
        display: flex;
        align-items: center;
        justify-content: space-between;
        border-radius: 5px;
        flex-wrap: wrap;
    }

    .task-content{
        font-size: 1rem;
    }

    .task-btn{
        border: none;
        outline: none !important;
        font-size: 1rem;
        background: none;
        cursor: pointer;
        padding: 0.25rem 0.25rem 0.25rem 0.75rem;
        margin-left: 0.25rem;
    }

    .task-edit-btn{
        color: steelblue;
    }

    .task-delete-btn{
        color: rgb(255, 41, 41);
    }

    .task-done-btn{
        color: springgreen;
        font-size: 1.25rem;
    }

    @media (max-width: 22.3125rem){
        
        .task{
            display: block;
        }

        .task-content{
            padding-top: 0.8rem;
            text-align: center;
        }

        .task-actions{
            text-align: center;
        }

        .task-btn{
            padding: 0.9rem 0.8rem;
        }

    }

</style>