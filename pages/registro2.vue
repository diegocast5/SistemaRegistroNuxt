<template>
    <div class="container">
        <nuxt-link to="/" tag="button" class="btn btn-secondary mt-3">Regresar al Inicio</nuxt-link>
        <b-form @submit.prevent="onSubmit">
            <b-form-group id="input-group-1" label="Dirección:" label-for="input-1">
                <b-form-input id="input-1" v-model="form.direccion" required
                    placeholder="Ingresa tu dirección"></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-2" label="Teléfono:" label-for="input-2">
                <b-form-input id="input-2" v-model="form.telefono" required
                    placeholder="Ingresa tu número de teléfono"></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-3" label="Ciudad:" label-for="input-3">
                <b-form-input id="input-3" v-model="form.ciudad" required placeholder="Ingresa tu ciudad"></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-4" label="Estado:" label-for="input-4">
                <b-form-input id="input-4" v-model="form.estado" required placeholder="Ingresa tu estado"></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-5" label="País:" label-for="input-5">
                <b-form-input id="input-5" v-model="form.pais" required placeholder="Ingresa tu país"></b-form-input>
            </b-form-group>

            <b-button type="submit" variant="primary">Registrarse</b-button>
        </b-form>
    </div>
</template>
  
<script lang="ts">
import axios from 'axios';
import Vue from 'vue'
import Toasted from 'vue-toasted'

Vue.use(Toasted)

export default {
    props: {
        cliente_id: {
            type: Number,
            default: function () {
                return this.$route.params.id;
            }
        }
    },
    data() {
        return {
            form: {
                direccion: '',
                telefono: '',
                ciudad: '',
                estado: '',
                pais: '',
            },
        }
    },
    methods: {
        async onSubmit() {
            const datosContacto = {
                datosContacto: {
                    cliente_id: this.cliente_id,
                    direccion: this.form.direccion,
                    telefono: this.form.telefono,
                    ciudad: this.form.ciudad,
                    estado: this.form.estado,
                    pais: this.form.pais,
                }
            }

            try {
                console.log(datosContacto.datosContacto);
                const response = await axios.post('http://localhost:8081/datos_contacto', datosContacto);
                console.log(response.data);

                let toast = this.$toasted.success('Datos de contacto registrados con éxito, recibira un correo de confirmacion.');
                setTimeout(() => {
                    toast.goAway(0);
                    this.$router.push('/');
                }, 3000);
            } catch (error) {
                console.error('Hubo un error al enviar los datos de contacto al servidor:', error);
                this.$toasted.error('Hubo un error al registrar los datos de contacto. Por favor, inténtalo de nuevo.');
            }
        }
    },
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