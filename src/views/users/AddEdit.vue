<script setup>
import { Form, Field } from 'vee-validate';
import * as Yup from 'yup';
import { useRoute } from 'vue-router';
import { storeToRefs } from 'pinia';

import { useUsersStore, useAuthStore } from '@/stores';
import { router } from '@/router';

const authStore = useAuthStore();
const usersStore = useUsersStore();
const route = useRoute();
const id = route.params.id;

let title = 'Add User';
let user = null;
if (id) {
    // edit mode
    title = 'Edit User';
    ({ user } = storeToRefs(usersStore));
    usersStore.getById(id);
}

const schema = Yup.object().shape({
    fullName: Yup.string()
        .required('First Name is required'),
    email: Yup.string()
        .required('Last Name is required'),
    username: Yup.string()
        .required('Username is required'),
    password: Yup.string()
        .transform(x => x === '' ? undefined : x)
        .concat(user ? null : Yup.string().required('Password is required'))
        .min(6, 'Password must be at least 6 characters'),
    userrole: Yup.number()
        .transform(x => x === 0 ? undefined : x)
        .min(0, 'Role must be >= 0 & <=2')
        .max(2,'Role must be >= 0 & <=2')
});

async function onSubmit(values) {
    try {
        let message;
        if (user) {
            values.id = user.value.id;
            values.userRole = Number(values.userRole ?? "2");
            await usersStore.update(user.value.id, values)
            message = 'User updated';
        } else {
            await authStore.register(values);
            message = 'User added';
        }
        await router.push('/users');
    } catch (error) {
        console.log(error)
    }
}
</script>
    
<template>
    <h1>{{title}}</h1>
    <template v-if="!(user?.loading || user?.error)">
        <!-- <Form @submit="onSubmit" :validation-schema="schema" :initial-values="user" v-slot="{ errors, isSubmitting }"> -->
        <Form @submit="onSubmit" :initial-values="user" v-slot="{ errors, isSubmitting }">
            <div class="form-row">
                <div class="form-group col">
                    <label>Full Name</label>
                    <Field name="fullName" type="text" class="form-control"
                        :class="{ 'is-invalid': errors.fullName }" />
                    <div class="invalid-feedback">{{ errors.fullName }}</div>
                </div>
                <div class="form-group col">
                    <label>Email</label>
                    <Field name="email" type="text" class="form-control"
                        :class="{ 'is-invalid': errors.email }" />
                    <div class="invalid-feedback">{{ errors.email }}</div>
                </div>
            </div>
            <div class="form-row">
                <div class="form-group col">
                    <label>Username</label>
                    <Field name="userName" type="text" class="form-control"
                        :class="{ 'is-invalid': errors.userName }" />
                    <div class="invalid-feedback">{{ errors.userName }}</div>
                </div>
                <div class="form-group col">
                    <label>
                        Password
                        <em v-if="user">(Leave blank to keep the same password)</em>
                    </label>
                    <Field name="password" type="password" class="form-control"
                        :class="{ 'is-invalid': errors.password }" />
                    <div class="invalid-feedback">{{ errors.password }}</div>
                </div>
            </div>
            <div v-if = "authStore.user.userRole == 0" class="form-row">
                <div class="form-group col">
                    <label>Role</label>
                    <Field name="userRole" type="text" class="form-control"
                        :class="{ 'is-invalid': errors.userRole }" />
                    <div class="invalid-feedback">{{ errors.userRole }}</div>
                </div>
            </div>
            <div class="form-group">
                <button class="btn btn-primary" :disabled="isSubmitting">
                    <span v-show="isSubmitting" class="spinner-border spinner-border-sm mr-1"></span>
                    Save
                </button>
                <router-link to="/users" class="btn btn-link">Cancel</router-link>
            </div>
        </Form>
    </template>
    <template v-if="user?.loading">
        <div class="text-center m-5">
            <span class="spinner-border spinner-border-lg align-center"></span>
        </div>
    </template>
    <template v-if="user?.error">
        <div class="text-center m-5">
            <div class="text-danger">Error loading user: {{user.error}}</div>
        </div>
    </template>
</template>