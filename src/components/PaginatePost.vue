<script setup>
defineProps (["inicio", "fin", "finarray"])
const emit= defineEmits(["prev", "next"])
</script>

<template>
   <div class="btn-group" role="group" aria-label="Basic example">
      <button 
         @click= "emit ('prev')" 
         type="button" 
         class="btn btn-primary"
         :disabled="inicio <= 0"
         >
         Previus {{inicio}}
      </button>
      <button 
         @click= "emit ('next')" 
         type="button" 
         class="btn btn-primary"
         :disabled="fin >= finarray"
         >Next {{ fin }}
      </button>
   </div>
</template>