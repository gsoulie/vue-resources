[< Back to main Menu](https://github.com/gsoulie/vue-resources/blob/main/vue-index.md)    

# Appels Http

* [Axios](#axios)    

## Axios 

**Axios** est une librairie permettant de réaliser des appels http

````npm i axios````

*Home.vue*
````typescript
<script>
	import axios from 'axios
	export default {
		setup() {
			data = ref([])
			
			onMounted(async () => {	// <--- ne pas oublier le async
				fetchData();
			})
			
			async function fetchData() {
				try {
					const { data } = await axios.get('https://dummyjson.com/products');
            				console.log(data);
				} catch(e) {
				
				}
			}
			
			const createPost = () => {
				axios.post('https://dummyjson.com/products/add',
					JSON.stringify(
						id: 99,
						title: 'akdapdokzap',
						...
					)
				)
				.then(response => {
					console.log(response);
				})
				.catch(e => console.warn(e));
			}
		}
	}
</script>
````

````axios.get()```` retourne une promise

[Back to top](#appels-http)     
