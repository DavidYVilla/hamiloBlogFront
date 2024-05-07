<template>
    <!-- Page Header Start -->
    <div class="container py-5 px-2 bg-primary">
        <div class="row py-5 px-4">
            <div class="col-sm-6 text-center text-md-left">
                <h1 class="mb-3 mb-md-0 text-white text-uppercase font-weight-bold">Categorías </h1>
            </div>
            <div class="col-sm-6 text-center text-md-right">
                <div class="d-inline-flex pt-2">
                    <h4 class="m-0 text-white">
                        <!-- <a class="text-white" href="#">Home</a> -->
                        <router-link to="/" class="text-white">Inicio</router-link>
                    </h4>
                    <h4 class="m-0 text-white px-2">/</h4>
                    <h4 class="m-0 text-white">Categorías</h4>
                </div>
            </div>
        </div>
    </div>
    <!-- Page Header End -->
    <div class="container py-5 px-2 bg-white">
        <div class="row px-4">
            <div v-for="item in categorias" :key="item.id" class="col-md-6">
                <div @click="filtrar(item.nombre)" class="card bg-dark text-white m-1">
                    <img :src="item.imagen" class="card-img" alt="...">
                    <div class="card-img-overlay text-center">
                        <br><br>
                        <h2 class="card-title text-dark p-3 shadow-lg">
                            <b>{{ item.nombre }}</b>
                        </h2>
                        <p class="card-text">
                            <i class="fas fa-list    "></i> {{ item.cant_posts }} Post
                        </p>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <div>
        dsfsdfvsdfw
    </div>
</template>
<script>

import { onMounted, ref } from 'vue'
import axios from 'axios';
import { useRouter } from 'vue-router';

export default {
    setup() {
        const router = useRouter();

        const categorias = ref([]);
        const baseUrl = 'http://hamiloblog.test/api'

        onMounted(() => {
            obtenerDatos();
        });
        const obtenerDatos = async () => {
            try {
                const { data } = await axios.get(baseUrl + '/categorias');

                categorias.value = data.datos;

            } catch (error) {
                console.log(error);
            }
        }
        const filtrar = (id) => {
            router.push({ path: '/blog/' + id });
        }
        return {
            categorias,
            filtrar

        }
    }
}


</script>

<style scoped></style>
