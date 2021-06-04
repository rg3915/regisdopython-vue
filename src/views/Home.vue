<template>
    <div>
        <div
            v-for="user in users"
            :key="user.id"
            class="card m-3"
        >
            <div class="card-body">
                <router-link
                    :to="{ name: 'user-todo', params: { id: user.id }}"
                >
                    {{ user | fullName }}
                </router-link>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Home',
    data() {
        return {
            users: [],
        };
    },
    mounted() {
        fetch('http://localhost:8000/api/v1/core/users')
            .then(response => response.json())
            .then((res) => {
                this.users = res; // .data foi removido
            });
    },
    filters: {
        fullName(user) {
            return user.first_name + ' ' + user.last_name
        }
    }
}
</script>
