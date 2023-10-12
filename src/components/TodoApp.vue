<template>
<div class="container">
    <h2>My Vue Todo List</h2>
    <!-- Input -->
    <div class="d-flex">
        <input v-model="task" type="text" placeholder="Enter Task" class="form-control" />
        <button @click="submitTask" class="btn">SUBMIT</button>
    </div>

    <!-- Task Table -->
    <table class="todo-table">
        <thead>
            <tr>
                <th>Task</th>
                <th>Status</th>
                <th>Edit</th>
                <th>Delete</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="(task, index) in tasks" :key="index">
                <td>{{ firstCharUpper(task.name) }}</td>
                <td class="pointer" :class="getTaskStatusClass(task.status)" @click="changeStatus(index)">
                    {{ firstCharUpper(task.status) }}
                </td>
                <td class="pointer" @click="editTask(index)">&#9998;</td>
                <td class="pointer" @click="deleteTask(index)">&#128465;</td>
            </tr>
        </tbody>
    </table>

    <!-- Finished Tasks Table -->
    <h3 style="color: green;">Completed Tasks</h3>
    <ul>
        <li v-for="(task, index) in finishedTasks" :key="index">{{ firstCharUpper(task.name) }}</li>
    </ul>

    <!-- Incompleted-Tasks Tasks Table -->
    <h3 style="color: orangered;">Incomplete Tasks</h3>

    <ul>
        <li v-for="(task, index) in inProgressTasks" :key="index">{{ firstCharUpper(task.name) }}</li>
    </ul>

</div>
</template>

<script>
export default {

    name: 'TodoApp',
    computed: {
        finishedTasks() {
            return this.tasks.filter(task => task.status === 'Completed-Tasks');
        },
        inProgressTasks() {
            return this.tasks.filter(task => task.status === 'Incompleted-Tasks');
        },
    },
    props: {
        message: String,
    },

    data() {
        return {
            availableStatuses: ['Task', 'Completed-Tasks', 'Incompleted-Tasks'],
            editedTask: null,
            task: '',
            tasks: [{
                name: 'Coding',
                status: 'Task',
            }, ],
        };
    },
    methods: {
        // 
        submitTask() {
            if (this.task.trim() === '') {
                // Alert the user if the task title is empty
                alert('Task title cannot be empty.');
                return;
            }

            if (this.editedTask === null) {
                this.tasks.push({
                    name: this.task,
                    status: 'Task',
                });
            } else {
                this.tasks[this.editedTask].name = this.task;
                this.editedTask = null;
            }

            this.task = '';
        },
        //Delete button
        deleteTask(index) {
            this.tasks.splice(index, 1);
        },
        //Edit button
        editTask(index) {
            this.task = this.tasks[index].name;
            this.editedTask = index;
        },
        changeStatus(index) {
            let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
            if (++newIndex > 2) newIndex = 0;
            this.tasks[index].status = this.availableStatuses[newIndex];
        },
        firstCharUpper(str) {
            return str.charAt(0).toUpperCase() + str.slice(1);
        },
        getTaskStatusClass(status) {
            return {
                'text-danger': status === 'Task',
                'text-warning': status === 'Incompleted-Tasks',
                'text-success': status === 'Completed-Tasks',
            };
        },
    },
};
</script>

<style scoped>
.container {
    text-align: center;
    font-family: Arial, sans-serif;
    animation: slideInRight 0.5s ease-in-out;
}

input.form-control {
    padding: 10px;
    width: 70%;
    border: 1px solid #ccc;
    border-radius: 5px;
}

button.btn {
    padding: 10px 20px;
    background-color: #ffaa00;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.2s ease-in-out;
}

button.btn:hover {
    background-color: #ff8800;
    transition: background-color 0.2s ease-in-out;
}

.todo-table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 20px;
    animation: slideInRight 0.5s ease-in-out;
}

.todo-table th,
.todo-table td {
    border: 1px solid #ddd;
    padding: 10px;
}

ul {
    list-style: none;
    padding: 0;
}

ul li {
    padding: 10px 0;
    border-bottom: 1px solid #ddd;
    animation: slideInRight 0.5s ease-in-out;
}

h3.task-list-title {
    font-size: 1.5rem;
    margin-top: 20px;
    animation: slideInRight 0.5s ease-in-out;
}

h3.completed-tasks {
    color: green;
}

h3.incomplete-tasks {
    color: orangered;
}

.pointer {
    cursor: pointer;
}

@keyframes slideInRight {
    0% {
        transform: translateX(100%);
        opacity: 0;
    }

    100% {
        transform: translateX(0);
        opacity: 1;
    }
}

@keyframes slideOutRight {
    0% {
        transform: translateX(0);
        opacity: 1;
    }

    100% {
        transform: translateX(-100%);
        opacity: 0;
    }
}

@media screen and (min-width: 320px) and (max-width: 480px) {
    .form-control {
        width: 100%;
    }

    button.btn {
        width: 100%;
        margin-top: 10px;
        transition: background-color 0.2s ease-in-out;
    }

    .todo-table {
        font-size: 14px;
        animation: slideInRight 0.5s ease-in-out;
    }

    h3.task-list-title {
        font-size: 1.2rem;
        animation: slideInRight 0.5s ease-in-out;
    }
}

@keyframes slideInRight {
    0% {
        transform: translateX(100%);
        opacity: 0;
    }

    100% {
        transform: translateX(0);
        opacity: 1;
    }
}

@keyframes slideOutRight {
    0% {
        transform: translateX(0);
        opacity: 1;
    }

    100% {
        transform: translateX(-100%);
        opacity: 0;
    }
}

@media screen and (min-width: 481px) and (max-width: 768px) {
    .container {
        padding: 20px;
        animation: slideInRight 0.5s ease-in-out;
    }

    .form-control {
        width: 70%;
    }

    button.btn {
        width: 100%;
        margin-top: 10px;
        transition: background-color 0.2s ease-in-out;
    }

    .todo-table {
        font-size: 16px;
        animation: slideInRight 0.5s ease-in-out;
    }

    ul {
        padding: 0;
    }

    ul li {
        padding: 10px 0;
        animation: slideInRight 0.5s ease-in-out;
    }

    h3.task-list-title {
        font-size: 1.4rem;
        animation: slideInRight 0.5s ease-in-out;
    }
}

@keyframes slideInRight {
    0% {
        transform: translateX(100%);
        opacity: 0;
    }

    100% {
        transform: translateX(0);
        opacity: 1;
    }
}

@keyframes slideOutRight {
    0% {
        transform: translateX(0);
        opacity: 1;
    }

    100% {
        transform: translateX(-100%);
        opacity: 0;
    }
}

@media screen and (min-width: 769px) and (max-width: 1024px) {
    .container {
        padding: 30px;
        animation: slideInRight 0.5s ease-in-out;
    }

    .form-control {
        width: 50%;
    }

    button.btn {
        width: 30%;
        margin-top: 10px;
        transition: background-color 0.2s ease-in-out;
    }

    .todo-table {
        font-size: 18px;
        animation: slideInRight 0.5s ease-in-out;
    }

    ul {
        padding: 0;
    }

    ul li {
        padding: 10px 0;
        animation: slideInRight 0.5s ease-in-out;
    }

    h3.task-list-title {
        font-size: 1.6rem;
        animation: slideInRight 0.5s ease-in-out;
    }
}

@keyframes slideInRight {
    0% {
        transform: translateX(100%);
        opacity: 0;
    }

    100% {
        transform: translateX(0);
        opacity: 1;
    }
}

@keyframes slideOutRight {
    0% {
        transform: translateX(0);
        opacity: 1;
    }

    100% {
        transform: translateX(-100%);
        opacity: 0;
    }
}

@media screen and (min-width: 1025px) and (max-width: 1200px) {
    .container {
        padding: 40px;
        animation: slideInRight 0.5s ease-in-out;
    }

    .form-control {
        width: 40%;
    }

    button.btn {
        width: 20%;
        margin-top: 10px;
        transition: background-color 0.2s ease-in-out;
    }

    .todo-table {
        font-size: 20px;
        animation: slideInRight 0.5s ease-in-out;
    }

    ul {
        padding: 0;
    }

    ul li {
        padding: 10px 0;
        animation: slideInRight 0.5s ease-in-out;
    }

    h3.task-list-title {
        font-size: 1.8rem;
        animation: slideInRight 0.5s ease-in-out;
    }
}

@media screen and (min-width: 1201px) {
    .container {
        padding: 60px;
        animation: slideInRight 0.5s ease-in-out;
    }

    .form-control {
        width: 30%;
    }

    button.btn {
        width: 15%;
        margin-top: 10px;
        transition: background-color 0.2s ease-in-out;
    }

    .todo-table {
        font-size: 22px;
        animation: slideInRight 0.5s ease-in-out;
    }

    ul {
        padding: 0;
    }

    ul li {
        padding: 10px 0;
        animation: slideInRight 0.5s ease-in-out;
    }

    h3.task-list-title {
        font-size: 2rem;
        animation: slideInRight 0.5s ease-in-out;
    }
}
</style>
