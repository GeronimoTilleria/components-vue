<script setup>
// const props = defineProps(['title']);
// console.log(props);

// String, Number, Boolean, Array, Object, Date, Function, Symbol
defineProps({
    title: String,
    id: Number,
    body: {
        type: String,
        default: 'Sin descripción'
    },
});

const emit = defineEmits(["fijarFavorito"]);

</script>

<template>
    <!-- <h2>{{ title }}</h2> -->
    <div class="card">
        <div class="card-body">
            <h5 class="card-title">{{ id }} - {{ title }}</h5>
            <p>{{ body }}</p>
            <button
                class="btn btn-sm btn-outline-primary"
                @click="emit('fijarFavorito', title)"
            >
                Mi Favorito
            </button>
        </div>
    </div>
</template>