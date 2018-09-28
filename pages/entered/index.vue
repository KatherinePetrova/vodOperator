<template>
	<div class="back">
		<div class="no_access" v-if="auth">
			<nuxt-link class="logo" to="/"><h1>vodOperator</h1></nuxt-link>
			<form class="no_access">
				<label id="auth">Вы не авторизированы</label>
				<nuxt-link to="/login" class="submit">Перейти на страницу авторизации</nuxt-link>
			</form>
		</div>
		<div v-else class="main">
			<div class="left">
				<div class="left-box" style="min-height: 15vh; align-items: center; justify-content: center">
					<nuxt-link class="logo" to="/" style="margin-bottom: 0px"><h1>vodOperator</h1></nuxt-link>
				</div>
				<div class="left-box link">
					Заявки >
				</div>
			</div>
			<div class="right">

			</div>
		</div>
	</div>
</template>
<style type="text/css" scoped>
	.back {
		min-height: 100vh;
		background-color: rgb(245, 245, 245);
	}
	div.no_access {
		display: flex;
		align-items: center;
		justify-content: center;
		min-height: 100vh;
		flex-direction: column;
		background-color: rgb(245, 245, 245);
	}
	form.no_access {
		display: flex;
		flex-direction: column;
		align-items: center;
		border-style: solid;
		border-width: 0.01em;
		border-color: white;
		padding-left: 5em;
		padding-right: 5em;
		font-size: 1.5em;
		background-color: rgb(51, 255, 119);
		color: white;
	}
	.submit {
		margin-top: 1em;
		margin-bottom: 2.5em;
		padding: 1em 3em 1em 3em;
		transition: 1s;
		color: white;
		background: rgb(0, 153, 77);
		border-style: inherit;
		font-size: 0.75em;
		text-decoration: none;
	}

	.submit:hover {
		cursor: pointer;
		background: white;
		color: rgb(0, 153, 77);
	}
	.logo {
		margin-bottom: 1.5em;
		color: rgba(102, 255, 102, 0.8);
		text-shadow: 0 20px rgba(166, 166, 166, 0.8),
               		 0 -20px rgba(140, 255, 102, 0.8);
        transition: 1s;
        font-style: normal;
        text-decoration: none;
	}

	.logo:hover {
		text-shadow: 0 1.5px rgba(166, 166, 166, 0.8),
               		 0 -1.5px rgba(140, 255, 102, 0.8);
	}

	#auth {
		margin-top: 1em;
		margin-bottom: 1em;
	}

	div.main {
		display: flex;
		flex-direction: row;
		min-height: 100vh;
		background: black;
	}
	div.left {
		display: flex;
		min-width: 20vw;
		min-height: 100vh;
		background-color: rgb(245, 245, 245);
		flex-direction: column;
		border-right-style: solid;
		border-width: 1px;
		border-color: rgba(140, 255, 102, 0.8);
	}
	div.right {
		display: flex;
		min-width: 80vw;
		min-height: 100vh;
		background-color: white;
	}
	div.left-box {
		display: flex;
		width: 20vw;
		background-color: block;
		border-bottom-style: solid;
		border-color: rgba(140, 255, 102, 0.8);
		border-width: 1px;
		min-height: 10vh;
	}
	div.link {
		align-items: center;
		padding-left: 5vh;
		font-size: 1.25em;
		color: rgba(166, 166, 166, 0.8);
	}
</style>
<script type="text/javascript">
	import axios from 'axios';
	export default{
		data(){
			return{
				auth: true
			}
		},
		methods:{
			check(){
				axios
					.post('http://localhost:8000/users/check', {token: this.$cookies.get('token')})
					.then(response => {
						this.auth = false;
					})
					.catch(error => {
						this.auth = true;
					});
			}
		},
		mounted(){
			this.check();
		}
	}
</script>