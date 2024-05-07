<template>
    <!-- Page Header Start -->
    <div class="container py-5 px-2 bg-primary">
        <div class="row py-5 px-4">
            <div class="col-sm-6 text-center text-md-left">
                <h1 class="mb-3 mb-md-0 text-white text-uppercase font-weight-bold">Ingresar </h1>
            </div>
            <div class="col-sm-6 text-center text-md-right">
                <div class="d-inline-flex pt-2">
                    <h4 class="m-0 text-white">
                        <!-- <a class="text-white" href="#">Home</a> -->
                        <router-link to="/" class="text-white">Inicio</router-link>
                    </h4>
                    <h4 class="m-0 text-white px-2">/</h4>
                    <h4 class="m-0 text-white">Ingresar</h4>
                </div>
            </div>

        </div>
    </div>
    <!-- Page Header End -->
    <div v-show="tipo == 'registro'" class="container py-5 px-2 bg-white">
        <div class="row px-4">
            <div class="col-12">
                <div class="form-group">
                    <label for="name">Nombre *</label>
                    <input type="text" class="form-control" v-model="name">
                </div>
                <div class="form-group">
                    <label for="email">Email *</label>
                    <input type="email" class="form-control" v-model="email">
                </div>
            </div>
            <div class="col-md-6">
                <div class="form-group">
                    <label for="website">Contraseña</label>
                    <input type="password" class="form-control" v-model="password">
                </div>
            </div>
            <div class="col-md-6">
                <div class="form-group">
                    <label for="website">Confirmar Contraseña</label>
                    <input type="password" class="form-control" v-model="password_confirmation">
                </div>
            </div>
            <div class="col-12">
                <div class="form-group">
                    <button @click="registro()" type="button" class="btn btn-primary">Registrarse</button>
                    <br><br>
                    <p>Ya tienes una cuenta? <button @click="cambiarForm()" class="button btn-link">
                            Ingresar </button> </p>
                    <br><br>
                </div>
            </div>


        </div>
    </div>
    <div v-show="tipo == 'login'" class=" container py-5 px-2 bg-white">
        <div class="row px-4">
            <div class="col-12">

                <div class="form-group">
                    <label for="email">Email *</label>
                    <input type="email" class="form-control" v-model="email">
                </div>
                <div class="form-group">
                    <label for="website">Contraseña</label>
                    <input type="password" class="form-control" v-model="password">
                </div>
                <div class="col-12">
                    <div class="form-group">
                        <button @click="login()" type="button" class="btn btn-primary">Ingresar</button>
                    </div>
                    <br><br>
                    <p>No tienes una cuenta? <button @click="cambiarForm()" class="button btn-link">
                            Registrate </button> </p>
                </div>


            </div>
        </div>
    </div>
    <div>

    </div>
</template>
<script>

import { onMounted, ref } from 'vue'
import axios from 'axios';
import { useRouter } from 'vue-router';

export default {
    setup() {
        const router = useRouter();
        const baseUrl = 'http://hamiloblog.test/api'
        const tipo = ref('login');
        const name = ref('');
        const email = ref('');
        const password = ref('');
        const password_confirmation = ref('');



        const ultimoPost = ref(null);
        onMounted(() => {
            ultimoPost.value = localStorage.getItem('ultimoPost');
        })
        const cambiarForm = () => {
            if (tipo.value == 'login') {
                tipo.value = 'registro';
            } else {
                tipo.value = 'login';
            }
        }

        const registro = async () => {
            try {
                const { data } = await axios.post(baseUrl + '/register', {
                    name: name.value,
                    email: email.value,
                    password: password.value,
                    password_confirmation: password_confirmation.value,
                }, {
                    headers: {
                        'Content-Type': 'application/json',
                        'Accept': 'application/json',
                    }
                });
                console.log(data);
                localStorage.setItem('token', data.token);
                localStorage.setItem('usuario', JSON.stringify(data.usuario));
                if (ultimoPost.value != null && ultimoPost.value != undefined) {
                    router.push({ path: '/vermas/' + ultimoPost.value });
                } else {
                    router.push({ path: '/blog' });
                }
            } catch (error) {
                console.log(error);
            }
        }
        const login = async () => {
            try {
                const { data } = await axios.post(baseUrl + '/login', {
                    email: email.value,
                    password: password.value,

                }, {
                    headers: {
                        'Content-Type': 'application/json',
                        'Accept': 'application/json',
                    }
                });
                console.log(data);
                localStorage.setItem('token', data.token);
                localStorage.setItem('usuario', JSON.stringify(data.usuario));
                if (ultimoPost.value != null && ultimoPost.value != undefined) {
                    router.push({ path: '/vermas/' + ultimoPost.value });
                } else {
                    router.push({ path: '/blog' });
                }
            } catch (error) {
                console.log(error);
            }
        }

        return {
            tipo,
            name,
            email,
            password,
            password_confirmation,
            cambiarForm,
            registro,
            login,

        }
    }
}


</script>

<style scoped></style>
