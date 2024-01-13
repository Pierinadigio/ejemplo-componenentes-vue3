<script setup>

defineProps ({
    title: String,
    id: Number,
    body: {
        type: String,
        default: "Sin descripcion",
        
    },
})

const emit= defineEmits(["actualizar"])
</script>
<template>
    <div class="card">
        <div class="card-body">
           
            <h5 
                class="card-title" 
            > Post {{ id }} - {{title}}</h5>
            <p
            >{{body }}</p>
            <button 
                @click="emit('actualizar', id)" 
                class="btn btn-outline-primary"
            >
            Favorito
            </button>

        </div>

    </div>
</template>