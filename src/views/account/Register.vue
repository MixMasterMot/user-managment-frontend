<script setup>
import { Form, Field } from 'vee-validate';
import * as Yup from 'yup';

import { useAuthStore } from '@/stores';
import { router } from '@/router';

const schema = Yup.object().shape({
    fullName: Yup.string()
        .required('First Name is required'),
    email: Yup.string()
        .required('First Name is required'),
    username: Yup.string()
        .required('Username is required'),
    password: Yup.string()
        .required('Password is required')
        .min(6, 'Password must be at least 6 characters')
});

async function onSubmit(values) {
    const authStore = useAuthStore();
    try {
        await authStore.register(values);
        await router.push('/account/login');
    } catch (error) {
        console.log(error);
    }
}
</script>
    
<template>
    <div class="card m-3">
        <h4 class="card-header">Register</h4>
        <div class="card-body">
            <Form @submit="onSubmit" :validation-schema="schema" v-slot="{ errors, isSubmitting }">
                <div class="form-group">
                    <label>Full Name</label>
                    <Field name="fullName" type="text" class="form-control"
                        :class="{ 'is-invalid': errors.fullName }" />
                    <div class="invalid-feedback">{{ errors.fullName }}</div>
                </div>
                <div class="form-group">
                    <label>Email</label>
                    <Field name="email" type="text" class="form-control"
                        :class="{ 'is-invalid': errors.email }" />
                    <div class="invalid-feedback">{{ errors.email }}</div>
                </div>
                <div class="form-group">
                    <label>Username</label>
                    <Field name="username" type="text" class="form-control"
                        :class="{ 'is-invalid': errors.username }" />
                    <div class="invalid-feedback">{{ errors.username }}</div>
                </div>
                <div class="form-group">
                    <label>Password</label>
                    <Field name="password" type="password" class="form-control"
                        :class="{ 'is-invalid': errors.password }" />
                    <div class="invalid-feedback">{{ errors.password }}</div>
                </div>
                <div class="form-group">
                    <button class="btn btn-primary" :disabled="isSubmitting">
                        <span v-show="isSubmitting" class="spinner-border spinner-border-sm mr-1"></span>
                        Register
                    </button>
                    <router-link to="login" class="btn btn-link">Cancel</router-link>
                </div>
            </Form>
        </div>
    </div>
</template>