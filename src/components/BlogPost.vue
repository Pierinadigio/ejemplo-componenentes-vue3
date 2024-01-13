<script setup>
//defineProps(["colorText", "colorBody"]);
//le puedo pasar un objeto Props.. Debo poner que tipo de dato es la propiedad del objeto, Number, array, String, boolean , Function, Date, Object

defineProps ({
    title: String,
    colorText: String,
    body: {
        type: String,
        default: "Sin descripcion",
        
    },
    colorBody: String,
    
})
const emit= defineEmits(["actualizar"])
</script>
<template>
    <div class="card">
        <div class="card-body">
            <h5 
                class="card-title" 
                :class="`text-${colorText}`"
            >{{ title }}</h5>
            <p :class= "`text-${colorBody}`"
            >{{body }}</p>
            <button 
                @click="emit('actualizar', title)" 
                class="btn btn-outline-primary"
            >
            Agregar a Favoritos
            </button>

        </div>

    </div>
</template>