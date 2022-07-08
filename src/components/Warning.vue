<template>
   <div class="container" >
      <div class="warning" v-if="warning">
         <i class="warning__close fa-solid fa-rectangle-xmark fa-lg" @click="$emit('close')"></i>
         <h3 class="warning__title" :class="warning.type">{{warning.title}}</h3>
         <p class="warning__text">{{warning.text}}</p>
      </div>
   </div>   
</template>

<script>
export default {
   emits: ['close'],
   props: ['warning']
}
</script>