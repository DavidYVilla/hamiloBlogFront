<template>

    <!-- Page Header Start -->
    <div class="container py-5 px-2 bg-primary">
        <div class="row py-5 px-4">
            <div class="col-sm-6 text-center text-md-left">
                <h1 class="mb-3 mb-md-0 text-white text-uppercase font-weight-bold">Blog</h1>
            </div>
            <div class="col-sm-6 text-center text-md-right">
                <div class="d-inline-flex pt-2">
                    <h4 class="m-0 text-white">
                        <!-- <a class="text-white" href="#">Inicio</a> -->
                        <RouterLink to="/" class="text-white">Inicio</RouterLink>
                    </h4>
                    <h4 class="m-0 text-white px-2">/</h4>
                    <h4 class="m-0 text-white">Blog</h4>
                </div>
            </div>
        </div>
    </div>
    <!-- Page Header End -->

    <!-- Blog List Start -->
    <div v-if="loading == false" class="container bg-white pt-5">
        <div v-for="item in posts" :key="item.id" class="row blog-item px-3 pb-5">
            <div class="col-md-5">
                <img class="img-fluid mb-4 mb-md-0" :src="item.imagen" alt="Image">
            </div>
            <div class="col-md-7">
                <h3 class="mt-md-4 px-md-3 mb-2 py-2 bg-white font-weight-bold">{{ item.titulo }}</h3>
                <div class="d-flex mb-3">
                    <small class="mr-2 text-muted"><i class="fa fa-calendar-alt"></i> {{ item.fecha_publicacion
                        }}</small>
                    <small class="mr-2 text-muted"><i class="fa fa-folder"></i> {{ item.categoria.nombre }}</small>
                    <small class="mr-2 text-muted"><i class="fa fa-comments"></i> {{ item.cant_comentarios }}
                        Comentarios</small>
                </div>
                <p>
                    {{ item.resumen }}
                </p>
                <!-- <a class="btn btn-link p-0" href="#">Leer más <i class="fa fa-angle-right"></i></a> -->
                <RouterLink :to="'/vermas/' + item.id" class="btn btn-link p-0">
                    Leer más <i class="fa fa-angle-right"></i>
                </RouterLink>
            </div>
        </div>
    </div>
    <div v-else class="container bg-white p-5 text-center ">
        <i class="fas fa-spinner " style="font-size: 36px;"></i>
        cargando...
    </div>
    <!-- Blog List End -->

</template>

<script>
import axios from 'axios';
import { onMounted, ref } from 'vue'
import { useRouter } from 'vue-router';

export default {
    setup() {
        const router = useRouter();
        const posts = ref([]);
        const loading = ref(false);
        const baseUrl = 'http://hamiloblog.test/api';
        let elParametro = router.currentRoute.value.params.param;

        onMounted(() => {
            obtenerDatos();
        })

        const obtenerDatos = async () => {
            loading.value = true;
            try {
                const { data } = await axios.post(baseUrl + '/resultados', { busqueda: elParametro }, {
                    headers: {
                        'Content-Type': 'application/json',
                        'Accept': 'application/json'
                    }
                });
                posts.value = data.datos.data;
                console.log(data.datos);
                loading.value = false;
            } catch (error) {
                console.log(error);
            }
        }

        return {
            posts,
            loading,
        }
    }
}
</script>

<style scoped></style>