<script setup>
import { storeToRefs } from 'pinia';
import { Form, Field } from 'vee-validate';
import { useUsersStore, useAuthStore } from '@/stores';

const usersStore = useUsersStore();
const authStore = useAuthStore();
const { users } = storeToRefs(usersStore);

usersStore.getAll();

async function onSubmit(values) {
    await usersStore.search(values.search);
}
</script>
    
<template>
    <div>
        <h1>Users</h1>
        <Form @submit="onSubmit">
            <div class="form-row">
                <div class="form-group col">
                    <label>Search</label>
                    <Field name="search" type="text" class="form-control" />
                </div>
            </div>
        </Form>
        <router-link to="/users/add" class="btn btn-sm btn-success mb-2">Add User</router-link>
        <table class="table table-striped">
            <thead>
                <tr>
                    <th style="width: 30%">Full Name</th>
                    <th style="width: 30%">Email</th>
                    <th style="width: 30%">Username</th>
                    <th style="width: 10%"></th>
                </tr>
            </thead>
            <tbody>
                <template v-if="users.length">
                    <tr v-for="user in users" :key="user.id">
                        <td>{{ user.fullName }}</td>
                        <td>{{ user.email }}</td>
                        <td>{{ user.userName }}</td>
                        <td style="white-space: nowrap">
                            <router-link :to="`/users/edit/${user.id}`" class="btn btn-sm btn-primary mr-1">Edit
                            </router-link>
                            <button @click="usersStore.delete(user.id)" class="btn btn-sm btn-danger btn-delete-user"
                                :disabled="user.isDeleting">
                                <span v-if="user.isDeleting" class="spinner-border spinner-border-sm"></span>
                                <span v-else>Delete</span>
                            </button>
                        </td>
                    </tr>
                </template>
                <tr v-if="users.loading">
                    <td colspan="4" class="text-center">
                        <span class="spinner-border spinner-border-lg align-center"></span>
                    </td>
                </tr>
                <tr v-if="users.error">
                    <td colspan="4">
                        <div class="text-danger">Error loading users: {{users.error}}</div>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>