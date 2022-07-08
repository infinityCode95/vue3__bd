<template>
   <div class="container">
      <div class="people">
         <div v-if="people.length !== 0" class="people__list" v-for="person in people" :key="person.id">
            <h3 class="people__item">{{person.personName}}</h3>
            <button class="people__btn" @click="$emit('remove', person.id)">Удалить</button>
         </div>

         <div class="people__empty empty" v-else>
            <h3 class="empty__title">Нет данных</h3>
            <button class="empty__btn" @click="$emit('load')">Загрузить пользователей</button>
         </div>
      </div>
   </div>   
</template>

<script>
export default {
   emits: ['load', 'remove'],
   props: ['people']   
}
</script>