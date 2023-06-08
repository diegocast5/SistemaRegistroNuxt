<template>
    <div class="container">
        <nuxt-link to="/" tag="button" class="btn btn-secondary mt-3">Regresar al Inicio</nuxt-link>
        <b-form @submit.prevent="onSubmit">
            <b-form-group id="input-group-1" label="Nombre:" label-for="input-1">
                <b-form-input id="input-1" v-model="form.name" required placeholder="Ingresa tu nombre"></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-2" label="Correo Electrónico:" label-for="input-2">
                <b-form-input id="input-2" type="email" v-model="form.email" required
                    placeholder="Ingresa tu correo"></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-3" label="Contraseña:" label-for="input-3">
                <b-form-input id="input-3" type="password" v-model="form.password" required
                    placeholder="Ingresa tu contraseña"></b-form-input>
                <p class="error-message" v-if="passwordError">{{ passwordError }}</p>
                <p v-if="passwordStrengthMessage">{{ passwordStrengthMessage }}</p>
            </b-form-group>

            <b-form-group id="input-group-4" label="Confirmar Contraseña:" label-for="input-4">
                <b-form-input id="input-4" type="password" v-model="form.confirmPassword" required
                    placeholder="Confirma tu contraseña"></b-form-input>
            </b-form-group>

            <b-button type="submit" variant="primary">Sigueinte</b-button>
        </b-form>
    </div>
</template>
  
<script lang="ts">
import axios from 'axios';
import Vue from 'vue'
import Toasted from 'vue-toasted'

Vue.use(Toasted)

export default {
    data() {
        return {
            form: {
                name: '',
                email: '',
                password: '',
                confirmPassword: '',
            },
            passwordError: '',
            passwordStrengthMessage: ''
        }
    },
    methods: {
        async onSubmit() {
            // Validación de la contraseña
            if (this.form.password !== this.form.confirmPassword) {
                this.passwordError = 'Las contraseñas no coinciden';
                return;
            }

            const passwordRegex = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{10,15}$/;

            if (!passwordRegex.test(this.form.password)) {
                this.passwordError = 'La contraseña no cumple con los requisitos mínimos';
                return;
            }

            // Preparar datos del usuario para enviar al servidor
            const usuario = {
                cliente: {
                    nombre: this.form.name,
                    correo: this.form.email,
                    clave: this.form.password
                }
            }

            try {
                console.log(usuario);
                // Enviar datos del usuario al servidor
                const response = await axios.post('http://localhost:8081/usuarios', usuario);
                console.log(response.data);

                // Mostrar una notificación de éxito y mantener una referencia a ella
                let toast = this.$toasted.success('Se envio un mensaje de a su correo para confirmarfavor de validar.');
                // Redirigir al usuario a la página de registro2 después de 2 segundos y ocultar la notificación
                setTimeout(() => {
                    toast.goAway(0);
                    this.$router.push({ name: 'registro2', params: { id: response.data.id } });
                }, 2000);
            } catch (error) {
                console.error('Hubo un error al enviar los datos del usuario al servidor:', error);

                // Mostrar una notificación de error
                this.$toasted.error('Hubo un error al registrar. Por favor, inténtalo de nuevo.');
            }
        }
    },
    watch: {
        'form.password': function (newPassword) {
            this.passwordError = '';
            this.passwordStrengthMessage = '';

            const strongPasswordRegex = /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{10,15}$/;
            const mediumPasswordRegex = /^(?=.*[A-Za-z])(?=.*\d)[A-Za-z\d]{8,}$/;

            if (strongPasswordRegex.test(newPassword)) {
                this.passwordStrengthMessage = 'La seguridad de la contraseña es: Fuerte';
            } else if (mediumPasswordRegex.test(newPassword)) {
                this.passwordStrengthMessage = 'La seguridad de la contraseña es: Media';
            } else {
                this.passwordStrengthMessage = 'La seguridad de la contraseña es: Débil';
            }
        }
    }
}
</script>
  
<style scoped>
.error-message {
    color: red;
}

.container {
    max-width: 600px;
    margin: 0 auto;
    padding: 20px;
}

b-button {
    margin-top: 20px;
}
</style>
  