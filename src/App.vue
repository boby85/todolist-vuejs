<template>
    <div class="container">
        <div class="content">
            <div class="header">
                <h3>My to do list:</h3>
                <div class="row">
                    <div class="input-group mb-3">
                        <input type="text" class="form-control" placeholder="Enter your task here..." v-model.trim="inputTask">
                        <div class="input-group-append">
                            <button class="btn btn-primary" type="button" @click="addToList">Add task</button>
                        </div>
                    </div>
                </div>
                <div class="row">
                    <div class="errorMessage col" v-if="this.errorMessage">
                        {{ this.errorMessage }}
                    </div>
                </div>
            </div>
            <div class="main">
                <div v-for="task in tasks" class="d-flex justify-content-between">
                    <div @click="toggleTaskDone(task.id)" :class="task.finished ? 'taskFinished' : '' "> {{ task.name }}</div>
                    <div @click="deleteTask(task.id)">
                        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-trash" viewBox="0 0 16 16">
                        <path d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0V6z"/>
                        <path fill-rule="evenodd" d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1v1zM4.118 4 4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4H4.118zM2.5 3V2h11v1h-11z"/>
                    </svg>
                    </div>
                </div>
            </div>

        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            tasks: [],
            numberOfTasks: 0,
            inputTask: null,
            inputIsValid: true,
            errorMessage: null,
        }
    },
    methods: {
        validateInput() {
            if (this.inputTask.length <  3 || this.inputTask.length > 100) {
                this.inputIsValid = false;
                this.errorMessage = 'Please check your input';
            }
        },
        resetValidation() {
            this.inputIsValid = true;
            this.errorMessage = null;
        },
        addToList() {
            this.resetValidation();
            this.validateInput();
            if(!this.inputIsValid) {
                return;
            }
            this.tasks.push(
                {
                    id: this.numberOfTasks + 1,
                    name: this.inputTask,
                    finished: false
                }
            );
            this.numberOfTasks++;
            this.inputTask = null;
        },
        deleteTask(id) {
            let index = this.tasks.findIndex(task => task.id === id);
            this.tasks.splice(index, 1);
            this.numberOfTasks--;
        },
        toggleTaskDone(id) {
            let index = this.tasks.findIndex(task => task.id === id);
            this.tasks[index].finished = !this.tasks[index].finished;
        }
    }
}
</script>

<style scoped>
.container {
    max-width: 80%;
}
@media (min-width: 768px) {
    .container {
        max-width: 50%;
    }
}
.header {
    padding-bottom: 1rem;
}
h3 {
    padding: 1rem 1rem 1rem 0;
}
.errorMessage {
    color: red;
    font-size: 10px;
}
.taskFinished {
    text-decoration: line-through;
}
</style>
