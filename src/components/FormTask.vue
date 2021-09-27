<template>

    <template v-if="form.status == 'add'">
        
        <form @submit.prevent="addTask" class="tasks-form">

            <input v-model.trim="newTask" type="text" class="tasks-form-input" placeholder="Task...">

            <button class="tasks-form-btn">

                <i class="fas fa-plus"></i>

            </button>

        </form>
        <p class="form-error">{{formError}}</p>
        
    </template>

    <template v-else>

        <form @submit.prevent="updateTask" class="tasks-form">

            <input v-model.trim="newTask" type="text" class="tasks-form-input" placeholder="Task...">

            <button class="tasks-form-btn">

                <i class="fas fa-pen"></i>

            </button>

        </form>
        <p class="form-error">{{formError}}</p>

    </template>

</template>

<script>
import { ref, watch } from 'vue'

export default {
    props: ['form'],

    emits: ['addTask', 'updateTask'],

    setup(props, { emit }){
        // data
        const newTask = ref('')
        const formError = ref('')

        // methods
        const validate = () => {
            // new task be grand than 1 its valid
            if(newTask.value.length > 1){
                formError.value = ''
                return true
            }   
            else{
                // show error
                formError.value = 'Task is invalid'

                //after 2 soconds come hide error
                setTimeout(() => {
                    formError.value = ''
                }, 1500);

                return false
            }
        }

        const addTask = () => {
            // check task is valid or no
            const valid = validate()

            // if not be valid come back
            if(!valid) return

            // if be valid come go to tasks and add task
            emit('addTask', newTask.value)

            // reset form
            newTask.value = ''
        }   

        const updateTask = () => {
            // check task is valid or no
            const valid = validate()
            
            // if not be valid come back
            if(!valid) return

            // if be valid come go to tasks and add task
            emit('updateTask', newTask.value)

            // reset form
            newTask.value = ''
        }

        // watch
        watch(props.form, () => {
            // update form because changed form status
            if(props.form.status == 'edit'){
                newTask.value = props.form.data.content
            }
            
        })

        return { 
            newTask, formError,
            addTask, updateTask, validate 
        }
    }
}
</script>

<style scoped>

    .tasks-form{
        background: white;
        border-radius: 3px;
        padding: 1rem;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .tasks-form-input{
        width: 70%;
        box-shadow: 0 0 15px 5px rgba(0, 0, 0, 0.123);
        border-radius: 8px;
        height: 2.4rem;
        border: 1px solid #55bbf677;
        outline: none !important;
        font: inherit;
        text-indent: 10px;
        padding: 0.5rem;
    }

    .tasks-form-btn{
        border-radius: 8px;
        font-size: 1rem;
        display: flex;
        align-items: center;
        justify-content: center;
        border: none;
        outline: none !important;
        margin-left: 0.75rem;
        background: #55bcf6;
        color: white;
        cursor: pointer;
        padding: 0.6rem 1.1rem;
        box-shadow: 0 0 15px 5px #55bbf627;
    }

    .form-error{
        margin-bottom: 2rem;
        text-align: center;
        background: white;
        padding-bottom: 0.3rem;
        color: rgb(255, 55, 55);
        font-weight: 600;
        font-size: 0.95rem;
        border-radius: 0 0 3px 3px;
    }

    @media(max-width: 31.0625rem){
        
        .tasks-form{
            width: 95%;
            margin: auto;
        }

        .form-error{
            margin: 0 auto 2rem auto;
            width: 95%;
        }

    }

</style>