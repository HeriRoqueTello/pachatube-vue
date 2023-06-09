<template>
    <section class="p-6 h-[calc(100vh-68px)]">
        <div class="text-2xl text-white font-bold">
            <h1>{{ video.title }} | ID: {{ $route.params.id }}</h1>
        </div>
        <div class="text-center h-3/4 my-4">
            <iframe class="w-5/6 mx-auto h-full" :src="`https://www.youtube.com/embed/${video.url}`" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
        </div>
        <div>
            <h2 class="text-lg font-bold text-gray-300">{{ video.title }}</h2>
            <p class="text-sm text-white/40">0 visualizaciones</p>
            <p>{{ video.description }}</p>
        </div>
    </section>
</template>

<script>
    import axios from 'axios'
    export default {
        props: {
            id: {
                type: Number,
                required: true
            },
        },
        data() {
            return {
                video: {
                    title: '',
                    url: '',
                    description: ''
                }
            }
        },
        created() {
            this.getVideos()
        },
        methods: {
            async getVideos() {
                try {
                    axios.get(`${import.meta.env.VITE_URL_API}${this.id}`)
                        .then(response => {
                            // Cargar los datos existentes en las propiedades de datos de Vue
                            console.log(response.data);
                            this.video = response.data;
                    })
                        .catch(error => {
                            this.$router.push('/')
                            console.error(error);
                    });
                } catch (error) {
                    this.$router.push('/')
                    console.log('No hay nada que hacer aquí')
                }
            }
        },
        watch: {
            id() {
                this.getVideos()
            }
        }
    }
</script>

<style>
    @media (min-width: 1024px) {
        .about {
            min-height: 100vh;
            display: flex;
            align-items: center;
        }
    }
</style>
  