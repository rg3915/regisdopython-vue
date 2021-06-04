<template>
    <div class="about">
        <div class="d-flex justify-content-between">
            <div>
                <h1>{{ user.name }}</h1>
                <h6>{{ user.email }}</h6>
            </div>

            <div>
                <router-link :to="{ name: 'home'}">Voltar</router-link>
            </div>
        </div>

        <div
            v-for="todo in user.todos"
            :key="todo.id"
            class="card mb-3"
        >
            <div class="card-body">
                <div class="d-flex justify-content-between">
                    <div>
                        <h6 class="mb-0">{{ todo.title }}</h6>
                        <small class="text-muted">{{ todo.description }}</small>
                    </div>

                    <div>
                        <i
                            v-if="todo.is_done"
                            class="far fa-check-square is-link"
                            @click="toggleDone(todo, false)"
                        ></i>

                        <i
                            v-else
                            class="far fa-square is-link"
                            @click="toggleDone(todo, true)"
                        ></i>
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
            user: {},
        };
    },

    mounted() {
        this.getData();
    },
    methods: {
        getData() {
            const userId = this.$route.params.id;
            fetch(`http://localhost:8000/api/v1/core/users/${userId}`)
                .then(response => response.json())
                .then(res => this.user = res); // .data foi removido
        },
        toggleDone(obj, value) {
            const todoId = obj.id;
            fetch(`http://localhost:8000/api/v1/todo/todos/${todoId}/done`, {
                method: 'PUT',
                headers: {
                    'Content-Type': 'application/json'
                },
                body: JSON.stringify({
                    is_done: value
                })
            })
            .then(response => response.json())
            .then(() => {
                this.getData();
            })
        }
    }

};
</script>


<style scoped>
.is-link {
    cursor: pointer;
}
.is-link:hover {
    color: #0d6efd;
}
</style>