<template>
	<div class="back">
		<div class="modal" v-if="modal">
			<form class="no_access">
				<label id="auth">Отправка водителю</label>
				<select>
					<option>df</option>
				</select>
			</form>
		</div>
		<div class="no_access" v-if="auth">
			<nuxt-link class="logo" to="/"><h1>vodOperator</h1></nuxt-link>
			<form class="no_access">
				<label id="auth">Вы не авторизированы</label>
				<nuxt-link to="/login" class="submit">Перейти на страницу авторизации</nuxt-link>
			</form>
		</div>
		<div class="main" v-else >
			<div class="left">
				<div class="left-box" style="min-height: 15vh; align-items: center; justify-content: center">
					<nuxt-link class="logo" to="/" style="margin-bottom: 0px"><h1>vodOperator</h1></nuxt-link>
				</div>
				<div class="left-box link" v-bind:class="{act: act===0}" @click="switchTable(0)">
					Новые заявки >
				</div>
				<div class="left-box link" v-bind:class="{act: act===1}" @click="switchTable(1)">
					Принятые заявки >
				</div>
				<div class="left-box link" v-bind:class="{act: act===2}" @click="switchTable(2)">
					Водители >
				</div>
				<div class="left-box link exit" @click="logout()">
					Выход
				</div>
			</div>
			<div class="right">
				<div class="tables" v-if="act===0">
					<div class="row header">
						<div class="col">
							Адрес
						</div>
						<div class="col">
							Время поступления заявки
						</div>
						<div class="col">
							Статус
						</div>
						<div class="col">
							Отправить водителю
						</div>
					</div>
					<div class="row" v-for="item in new_app">
						<div class="col">
							{{ item.adress }}
						</div>
						<div class="col">
							{{ item.app_cometime }}
						</div>
						<div class="col">
							{{ item.name }}
						</div>
						<div class="col">
							<button class="col" @click="sendApp(item.id)">Отправить водителю</button>
						</div>
					</div>
				</div>
				<div class="tables" v-else-if="act===1">
					
				</div>
				<div class="tables" v-else-if="act===2">
					<div class="row header">
						<div class="col">
							Имя
						</div>
						<div class="col">
							Номер телефона
						</div>
						<div class="col">
							Марка автомобиля
						</div>
						<div class="col">
							Гос. номер
						</div>
						<div class="col">
							Статус
						</div>
					</div>
					<div class="row" v-for="item in free_driver">
						<div class="col">
							{{ item.name }}
						</div>
						<div class="col">
							{{ item.phone }}
						</div>
						<div class="col">
							{{ item.car }}
						</div>
						<div class="col">
							{{ item.car_number }}
						</div>
						<div class="col">
							Свободен
						</div>
					</div>
					<div class="row" v-for="item in busy_driver">
						<div class="col">
							{{ item.name }}
						</div>
						<div class="col">
							{{ item.phone }}
						</div>
						<div class="col">
							{{ item.car }}
						</div>
						<div class="col">
							{{ item.car_number }}
						</div>
						<div class="col">
							Выполняет заявку
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>
<style type="text/css" scoped>
	html, body {
		padding: 0;
		margin: 0;
		overflow: hidden;
	}
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
		border-width: 1px;
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
		position: relative;
	}
	div.right {
		display: flex;
		min-width: 80vw;
		min-height: 100vh;
		background-color: white;
		justify-content: center;
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
	div.link:hover {
		background-color: rgba(140, 255, 102, 0.8);
		color: white;
		cursor: pointer;
	}
	div.act {
		background-color: rgba(102, 255, 102, 0.8);
		color: white;
	}
	div.exit {
		background-color: rgba(255, 77, 77, 0.8);
		position: absolute;
		bottom: 0;
		color: white;
	}
	div.exit:hover {
		background-color: rgba(255, 77, 77, 1);
	}
	div.tables {
		display: flex;
		flex-direction: column;
		min-height: 100vh;
		min-width: 80vw;
		max-height: 100vh;
		overflow-y: auto;
	}
	div.row {
		display: flex;
		min-height: 10vh;
		justify-content: center;
		background-color: white;
		border-bottom-style: solid;
		border-color: rgba(166, 166, 166, 0.8);
		border-width: 1px;
	}
	div.header {
		font-weight: 500;
		background-color: rgb(240, 240, 240);
		
	}
	div.col {
		display: flex;
		height: 10vh;
		align-items: center;
		justify-content: center;
		width: 20vw;
		border-right-style: solid;
		border-color: rgba(166, 166, 166, 0.8);
		border-width: 1px;
		text-align: center;
	}
	button.col {
		display: flex;
		min-height: 100%;
		min-width: 100%;
		align-items: center;
		justify-content: center;
		background-color: rgba(102, 255, 102, 0.8);
		border-style: hidden;
		color: white;
		font-size: 1em;
		transition: 0.2s;
	}
	button.col:hover {
		cursor: pointer;
		background-color: rgba(140, 255, 102, 0.8);
	}
	button.col:hover {
		background-color: rgba(140, 255, 102, 0.8);
	}
	div.modal {
		position: absolute;
		z-index: 999;
		display: flex;
		align-items: center;
		justify-content: center;
		background-color: rgba(0, 0, 0, 0.5);
		min-height: 100vh;
		min-width: 100vw;
	}
</style>
<script type="text/javascript">
	import axios from 'axios';
	export default{
		data(){
			return{
				auth: true,
				modal: false,
				act: 0,
				new_app: [],
				ws: {},
				free_driver: [],
				busy_driver: []
			}
		},
		methods:{
			check(){
				axios
					.post('http://aida.market:8000/users/check', {token: this.$cookies.get('token')})
					.then(response => {
						this.auth = false;
					})
					.catch(error => {
						this.auth = true;
					});
			},
			switchTable(num){
				this.act = num;
			},
			logout(){
				this.$cookies.set('token', '');
				this.$router.push('/');
			},
			getData(){
				axios
					.post('http://aida.market:8000/get/new_app', {token: this.$cookies.get('token')})
					.then(response => {
						this.new_app = response.data;
						console.log(new_app);
					})
					.catch(error => {
						alert('Необходима авторизация');
					});

				axios
					.post('http://aida.market:8000/get/drivers', {token: this.$cookies.get('token')})
					.then(response => {
						var data = response.data;
						for(var i=0; i<data.length; i++){
							if(data.status){
								this.free_driver.push(data[i]);
							} else {
								this.busy_driver.push(data[i]);
							}
						}
					})
					.catch(error => {
						alert('Необходима авторизация');
					});
			},
			wss(){
				var self = this;
				var socket = new WebSocket("ws://aida.market:8001");
				socket.onmessage = function(event){
					var data = JSON.parse(event.data);
					console.log(JSON.parse(event.action));
					self.new_app.unshift(data);
				}
				this.ws = socket;
			},
			sendApp(id){
				this.modal = true;
			},
			wss(){
			}
		},
		mounted(){
			this.check();
			this.getData();
			this.wss();
		}
	}
</script>