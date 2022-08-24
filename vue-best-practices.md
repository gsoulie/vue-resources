[< Back to main Menu](https://github.com/gsoulie/vue-resources/blob/main/vue-index.md)    

# Bonnes pratiques

## ApiHelper

https://www.youtube.com/watch?v=fh0VboqAc8k&ab_channel=ProgramWithErik      

Créer un service ApiHelper

*apiHelper.ts*

````typescript
import axios from 'axios'

export default(url='http://api.kanye.rest') => {	//<-- url par défaut
	return axios.create({
		baseURL: url
	})
}
````

Ensuite créer autant de services que nécessaires pour chaque domaines

*KanyeAPI.ts*
````typescript
import apiHelper from './apiHelper'

export default {
	getQuote() {
		return apiHelper().get('/')
	}
	
	createPost(data) {
		return apiHelper('https://jsonplaceholder.typicode.com/').post('/posts', data)
	}
}
````

Appel depuis un composant

*Home.vue*

````typescript
import KanyeAPI from './services/KanyeAPI'

<script>
	export default {
		setup() {
			const quote = ref('');
			
			const loadQuote = async () => {
				const response = await KanyeAPI.getQuote()
				quote.value = response.data.quote
			}
			
			onMounted(async () => { loadQuote() })
			
			const createPost = async () => {
				const response = KanyeAPI.createPost(JSON.stringify({
					title: 'foo',
					body: 'bar',
					userId: 1
				}))
				.catch(e => console.warn(e))
				
				console.log(response)
			}
		}
	}
</script>
````

[Back to top](#bonnes-pratiques)     
