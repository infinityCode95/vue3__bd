<template>
<div class="container">
	<div class="app">
		<h1 class="app__title">Работа с базой данных</h1>

		<Warning :warning="warning" @close="warning = null"></Warning>

		<form class="app__form" @submit.prevent="createPerson">
			<input placeholder="Введите имя" class="app__input" type="text" id="name" v-model.trim="inputName">
			<button class="app__btn" :disabled="inputName.length === 0" >Создать</button>
		</form>

		<PeopleList :people="people" @load="loadPeople" @remove="removePerson"></PeopleList>
	</div>
</div>
</template>

<script>
import PeopleList from './components/PeopleList.vue'
import Warning from './components/Warning.vue'

import axios from 'axios'

export default {

	components: {PeopleList, Warning},

	data() {
		return {
			inputName: '',
			people: [],
			warning: null, 	
		}
 },

 mounted() {
	this.loadPeople()
 },
 
 methods: {
	async createPerson() {
		const response = await fetch('https://vue-http-60f9e-default-rtdb.firebaseio.com/people.json', {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json'
			},
			body: JSON.stringify({
				personName: this.inputName
			})
		})

		const firebaseData = await response.json()

		this.people.push({
			personName: this.inputName,
			id: firebaseData.name 	
		})

		this.inputName = ''
	},
	
	async loadPeople() {
		try {
			const {data} = await axios.get('https://vue-http-60f9e-default-rtdb.firebaseio.com/people.json')
			this.people = Object.keys(data).map(key => {
				return {
					id: key,
					personName: data[key].personName
				}
			})
		} catch (e) {
			this.warning = {
				type: 'error',
				title: 'Ошибка',
				text: 'Пользователи для загрузки отсутствуют',
			}	
		}
	},

	async removePerson(id) {

		try {
			const name = this.people.find(person => person.id === id).personName
			await axios.delete(`https://vue-http-60f9e-default-rtdb.firebaseio.com/people/${id}.json`)
			this.people = this.people.filter(person => person.id !== id)
			this.warning = {
				type: 'success',
				title: 'Успешно',
				text: `Пользователь ${name} удалён`
			}
		} catch (e) {
			
		}
	}
 }
}
</script>
