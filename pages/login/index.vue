<template>
	<div>
		<nuxt-link class="logo" to="/"><h1>vodOperator</h1></nuxt-link>
		<form v-if="show">
			<label id="auth">Вы вошли как {{ user }}</label>
			<nuxt-link to="/entered" class="submit">Продолжить</nuxt-link>
			<input type="button" value="Выход" @click="logout" class="submit_dis" style="margin-bottom: 2.5em">
		</form>
		<form v-on:submit.prevent="sendInfo" v-else>
			<label id="auth">Авторизация</label>
			<label class="labs mary" for="login">Логин</label>
			<input class="inp" type="text" id="login" name="login" v-model="login" ref="login" v-on:change="change" required>
			<label class="labs mary" for="password">Пароль</label>
			<input class="inp" type="password" id="password" name="password" v-model="pass" required>
			<input type="submit" class="submit" value="Войти" v-bind:disabled="dis" ref="sub">
			<nuxt-link to="/signin" style="margin-bottom: 2em; font-size: 0.75em; text-decoration: none">Регистрация</nuxt-link>
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

	.submit_dis:hover {
		cursor: pointer;
		background: white;
		color: rgb(255, 77, 77);
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
      		pass: '',
      		dis: false,
      		show: false,
      		user: ''
      	}
    },
    methods:{
    	sendInfo(){
      		axios
        	.post('http://localhost:8000/users/login', {login: this.login, pass:this.pass})
        	.then(response => {
          		let data = response;
          		this.$cookies.set('token', data.data);
          		this.$router.push('/entered');
        	})
        	.catch(error => alert("Неверный пароль"));
      	},
      	change(){
			axios
				.post('http://localhost:8000/users/compare', {login: this.login})
				.then(response => {
					this.$refs.login.style.backgroundColor = 'rgb(179, 255, 153)';
					this.$refs.sub.setAttribute('class', 'submit');
					this.dis = false;
				})
				.catch(error => {
					this.$refs.login.style.backgroundColor = 'rgb(255, 179, 179)';
					this.$refs.sub.setAttribute('class', 'submit_dis');
					this.dis = true;
				});
		},
		check(){
			axios
				.post('http://localhost:8000/users/check', {token: this.$cookies.get('token')})
				.then(response => {
					this.show = true;
					this.user = response.data;
				})
				.catch(error => {
					this.show = false;
				});
		},
		logout(){
			this.$cookies.set('token', '');
			this.$router.push('/');
		}
    },
    mounted(){
    	this.check();
    }
}
</script>