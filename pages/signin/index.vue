<template>
	<div>
		<nuxt-link class="logo" to="/"><h1>vodOperator</h1></nuxt-link>
		<form v-on:submit.prevent="sendInfo">
			<label id="auth">Регистрация</label>
			<label class="labs mary" for="login">Логин</label>
			<input class="inp" type="text" id="login" name="login" v-model="login" v-on:change="change()" ref="login" required>
			<label class="labs mary" for="email">E-mail</label>
			<input class="inp" type="email" id="email" name="mail" v-model="mail" required>
			<label class="labs mary" for="password">Пароль</label>
			<input class="inp" type="password" id="password" name="pass" v-model="pass" required>
			<input type="submit" class="submit" id="submit" v-bind:disabled="dis" value="Зарегестироваться" ref="sub">
			<nuxt-link to="/login" style="margin-bottom: 2em; font-size: 0.75em; text-decoration: none">Войти</nuxt-link>
		</form>
	</div>
</template>

<style type="text/css" scoped>
	div {
		display: flex;
		align-items: center;
		justify-content: center;
		min-height: 100vh;
		flex-direction: column;
		background-color: rgb(245, 245, 245);
	}
	form {
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

	.labs {
		font-size: 0.75em;
	}

	.mary {
		margin-top: 0.5em;
		margin-bottom: 0.5em;
	}

	#auth {
		margin-top: 1em;
		margin-bottom: 1em;
	}

	.submit {
		margin-top: 1em;
		margin-bottom: 1em;
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

	.submit_dis {
		margin-top: 1em;
		margin-bottom: 1em;
		padding: 1em 3em 1em 3em;
		transition: 1s;
		color: white;
		background: rgb(255, 77, 77);
		border-style: inherit;
		font-size: 0.75em;
		text-decoration: none;
	}

	.inp {
		font-size: 0.75em;
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
</style>

<script>
import axios from 'axios';
export default{
	data(){
    	return{
      		login: '',
      		mail: '',
      		pass: '',
      		dis: false
      	}
    },
    methods:{
    	sendInfo(){
      		axios
        	.post('http://aida.market:8000/users/new',{login: this.login, mail:this.mail, pass:this.pass})
        	.then(response => {
          		this.$router.push('/login');
        	})
        	.catch(error => alert(error));
      	},

      	change(){
			axios
				.post('http://aida.market:8000/users/compare', {login: this.login})
				.then(response => {
					this.$refs.login.style.backgroundColor = 'rgb(255, 179, 179)';
					this.$refs.sub.setAttribute('class', 'submit_dis');
					this.dis = true;
				})
				.catch(error => {
					this.$refs.login.style.backgroundColor = 'rgb(179, 255, 153)';
					this.$refs.sub.setAttribute('class', 'submit');
					this.dis = false;
				});
		}
    }
}
</script>