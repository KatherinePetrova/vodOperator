<template>
	<div>
		<h1>asasas</h1>
		<button @click="getCookie">go</button>
		<button @click="sendToken">token</button>
	</div>
</template>
<script type="text/javascript">
	import axios from 'axios';
	export default{
		data(){
			return{

			}
		},
		methods:{
			getCookie(){
				var x = this.$cookies.get('token')
				console.log(x);
			},
			sendToken(){
				axios
					.post('http://localhost:8000/users/check', {token: this.$cookies.get('token')})
					.then(response => {
						let data = response;
						console.log(data);
					})
					.catch(error => console.log(error));
			}
		}
	}
</script>