<template>
    <form>
        <v-container fluid>
            <v-row>

                <v-col cols="12">
                    <v-text-field
                        v-model="email"
                        :error-messages="emailErrors"
                        label="Email"
                        required
                        @blur="$v.email.$touch()"
                    ></v-text-field>
                </v-col>

                <v-col cols="12">
                    <v-text-field
                        v-model="password"
                        :error-messages="passwordErrors"
                        label="Password"
                        required
                        :append-icon="showPw ? 'mdi-eye' : 'mdi-eye-off'"
                        :type="showPw ? 'text' : 'password'"
                        @click:append="showPw = !showPw"
                        @blur="$v.password.$touch()"
                    ></v-text-field>
                </v-col>

                <v-col cols="12">
                    <v-checkbox
                        v-model="checkbox"
                        label="Stay logged in"
                    ></v-checkbox>
                </v-col>

                <v-col cols="12">
                    <v-btn block @click="submit">Log in</v-btn>
                </v-col>

            </v-row>
        </v-container>
    </form>
</template>

<script>
import { validationMixin } from 'vuelidate'
import { required, email } from 'vuelidate/lib/validators'

export default {
    mixins: [validationMixin],

    validations: {
        email: { required, email },
        password: { required },
    },

    data: () => ({
        email: '',
        password: '',
        showPw: false,
        checkbox: '',
    }),

    computed: {
        emailErrors () {
            const errors = []
            if (!this.$v.email.$dirty) return errors
            !this.$v.email.email && errors.push('Must be valid email')
            !this.$v.email.required && errors.push('Email is required')
            return errors
        },
        passwordErrors () {
            const errors = []
            if (!this.$v.password.$dirty) return errors
            !this.$v.password.required && errors.push('Password is required')
            return errors
        },
    },

    methods: {
        submit () {
            this.$v.$touch()
        },
    },
}
</script>
