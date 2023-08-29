<script setup>

import { onMounted, ref, computed } from "vue";

import BlogPost from "./components/BlogPost.vue";
import PaginatePosts from "./components/PaginatePosts.vue";
import LoadingSpinner from "./components/LoadingSpinner.vue";

const miFavorito = ref("");
const posts = ref([]);
const postXpage = 10;
const inicio = ref(0);
const fin = ref(postXpage);
const loading = ref(false);

onMounted(async () => {
    loading.value = true;
    try {
        const res = await fetch("https://jsonplaceholder.typicode.com/posts");
        posts.value = await res.json();
    } catch (error) {
        console.log(error);
    } finally {
        setTimeout(() => (loading.value = false), 1500);
    }
});

const fijarFavorito = (title) => {
    miFavorito.value = title;
};

const next = () => {
    inicio.value = inicio.value + postXpage;
    fin.value = fin.value + postXpage;
};

const prev = () => {
    inicio.value = inicio.value - postXpage;
    fin.value = fin.value - postXpage;
};

const maxLength = computed(() => posts.value.length);

const paginatePage = computed(() => posts.value.slice(inicio.value, fin.value));
</script>

<template>
    <LoadingSpinner v-if="loading" />
    <div class="container">
        <h1>{{ miFavorito || "Sin favorito" }}</h1>

        <PaginatePosts
            @next="next"
            @prev="prev"
            :inicio="inicio"
            :fin="fin"
            :maxLength="maxLength"
            class="mb-2"
        ></PaginatePosts>

        <BlogPost
            v-for="post in posts.slice(inicio, fin)"
            :key="post.title"
            :title="post.title"
            :id="post.id"
            :body="post.body"
            class="mb-2"
            @fijarFavorito="fijarFavorito"
        >
        </BlogPost>
    </div>
</template>