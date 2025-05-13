## Uniapp Development Tutorial for HarmonyOS Applications - Optional api and Composite api

Hello everyone. A couple of days ago, I talked about the basic layout and custom navigation bar when developing HarmonyOS applications through Uniapp. Both of these were limited to the development on the page. Today, we attempt to perform some data operations to share the contents of the optional API and the composite API.

The script code part of the homepage project code we initialized is usually like this:

```
<script>
	export default {
		data() {
			return {
				title: 'Hello',

			}
		},
		onLoad() {
		},
		methods: {

		}
	}
</script>
```

It can be seen that it is divided into three parts. As the name suggests, the data part is used to store data, onLoad is the event executed when the page is loaded, and methods is used to store event methods.

Write a simple demo: Define a variable 'age' with an initial value of 18, and then write a click event to modify the value of 'age'.

We already know the functions of the three parts in the script code. Therefore, the code for defining variables should be written in data, and the modification methods should be written in methods, like this:
```
<script>
	export default {
		data() {
			return {
				title: 'Hello',
				age:18

			}
		},
		onLoad() {
		},
		methods: {

			changeAge(){
				this.age = 20
			}

		}
	}
</script>
```
This coding method with clear functional distinctions is called optional api in Vue. Its advantages are obvious: clear partitioning and easy understanding. However, in large-scale projects, You LAN Jun generally doesn't use this approach. I prefer a flexible and free writing style. Therefore, I will choose another composite api, which has the same functions as before. The code of the composite api is as follows:
```
<script>
	import {ref} from 'vue'
	export default{
		setup() {
			const age = ref(18)

			const changeAge = ()=>{				
				age.value = 20
			}

			return { age,changeAge }
		}
	}
</script>
```


It seems that this way of writing is not much better than the previous one. The code is not simple. Since You LAN Jun likes to use it, it must have its unique features. Yes, there is also a simplified version of it. Just write the setup into the script tag, and all the code will become extremely flexible:

```
<script setup>
	import {ref} from 'vue'

	const age = ref(18)

	function changeAge(){
		age.value = 20
	}

</script>
```

Doesn't this kind of code look much more comfortable? So don't be surprised to see such code in the articles from today on. When you use it yourself, also pay attention to the details and don't forget the "setup" in the tag.
