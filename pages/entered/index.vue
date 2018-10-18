<template>
	<div class="back">
		<div class="img" v-if="img.open">
			<h1 class="img" @click="img.open=false">Закрыть</h1>
			<div class="carousel-wrapper">
				<input type="radio" checked="checked" id="slide1" class="carousel-selector" name="carousel-selector" />
				<input type="radio" id="slide2" class="carousel-selector" name="carousel-selector" />
				<input type="radio" id="slide3" class="carousel-selector" name="carousel-selector" />
				<input type="radio" id="slide4" class="carousel-selector" name="carousel-selector" />

				<ul class="carousel-items">
					<li class="carousel-item" v-for="item in img.srcs">
						<img v-bind:src="item" height="100%">
					</li>
				</ul>

				<ul class="carousel-labels">
					<li class="carousel-label">
						<label for="slide1"></label>
					</li>
					<li class="carousel-label">
						<label for="slide2"></label>
					</li>
					<li class="carousel-label">
						<label for="slide3"></label>
					</li>
					<li class="carousel-label">
						<label for="slide4"></label>
					</li>
				</ul>
			</div>
		</div>
		<div class="no_access" v-if="auth">
			<nuxt-link class="logo" to="/"><h1>vodOperator</h1></nuxt-link>
			<form class="no_access">
				<label id="auth">Вы не авторизированы</label>
				<nuxt-link to="/login" class="submit">Перейти на страницу авторизации</nuxt-link>
			</form>
		</div>
		<div class="img" v-if="new_app.open">
			<form v-on:submit.prevent="sendApp" class="no_access">
				<label id="auth">Новая заявка</label>
				<label for="nat">Адрес</label> 
				<input type="text" v-model="new_app.adress" id="nat" style="width: 100%" required>
				<select v-model="new_app.area" style="width: 100%; margin-top: 1vh" required>
					<option value="1">Город</option>
					<option value="2">Загород</option>
				</select>
				<input type="submit" value="Отправить" class="submit">
				<button class="col dec" @click="new_app.open=false">Отмена</button>
			</form>
		</div>
		<div class="main" v-else >
			<div class="left">
				<div class="left-box" style="min-height: 15vh; align-items: center; justify-content: center">
					<nuxt-link class="logo" to="/" style="margin-bottom: 0px"><h1>vodOperator</h1></nuxt-link>
				</div>
				<div class="left-box link" v-bind:class="{act: act===0}" @click="switchTable(0)">
					Заявки >
				</div>
				<div class="left-box link" v-bind:class="{act: act===2}" @click="switchTable(2)">
					Водители >
				</div>
				<div class="left-box link" @click="new_app.open=true">
					Новая заявка >
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
							Водитель
						</div>
						<div class="col">
							Время поступления заявки
						</div>
						<div class="col">
							Время начала выполнения заявки
						</div>
						<div class="col">
							Время окончания заявки
						</div>
						<div class="col">
							Комиссия
						</div>
						<div class="col">
							Доля водителя
						</div>
						<div class="col">
							Статус
						</div>
					</div>
					<div class="row" v-for="item in app">
						<div class="col">
							{{ item.adress }}
						</div>
						<div class="col">
							{{ getDriverName(item.driver) }}
						</div>
						<div class="col">
							{{ timeParse(item.app_cometime) }}
						</div>
						<div class="col">
							{{ timeParse(item.app_start) }}
						</div>
						<div class="col">
							{{ timeParse(item.app_finish) }}
						</div>
						<div class="col">
							{{ item.amount }}
						</div>
						<div class="col">
							{{ item.driver_amount }}
						</div>
						<div class="col">
							{{ app_status[parseInt(item.status)-1].name }}
						</div>
					</div>
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
							Баланс
						</div>
						<div class="col">
							Докумены
						</div>
						<div class="col">
							Статус
						</div>
					</div>
					<div class="row" v-for="item in driver">
						<div class="col">
							{{ item.name }}
						</div>
						<div class="col">
							{{ item.phone }}
						</div>
						<div class="col">
							{{ item.balance }}
						</div>
						<div class="col">
							<button class="col" @click="openImages(item)">Смотреть</button>
						</div>
						<div class="col" v-if="item.status==1 && item.acceptance==1">
							Свободен
						</div>
						<div class="col" v-else-if="item.status==0 && item.acceptance==1">
							Занят
						</div>
						<div class="col" v-else-if="item.acceptance==0">
							<button class="col" @click="accept(item, true)" style="min-width:50%">Подтвердить</button>
							<button class="col dec" @click="accept(item, false)" style="min-width:50%">Отклонить</button>
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
		justify-content: center;
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
		flex-direction: column;
	}
	button.col:hover {
		cursor: pointer;
		background-color: rgba(140, 255, 102, 0.8);
	}
	button.dec {
		background-color: rgba(255, 77, 77, 0.8);
	}
	button.dec:hover {
		background-color: rgba(255, 77, 77, 1);
	}
	div.img {
		position: absolute;
		display: flex;
		justify-content: center;
		align-items: center;
		background-color: rgba(0, 0, 0, 0.5);
		z-index: 5;
		min-height: 100vh;
		min-width: 100vw;
		flex-direction: column;
	}
	h1.img {
		margin-bottom: 10px;
		color: rgba(255, 77, 77, 0.8);
	}
	h1.img:hover {
		cursor: pointer;
		color: rgba(255, 77, 77, 1);
	}
	.carousel-items, .carousel-labels {
		margin: 0;
		padding: 0;
		list-style: none;
		display: block;
	}
	.carousel-item {
		display: block;
		float: left;
	}

	.carousel-wrapper {
		width: 80%;
		position: relative;
		padding-bottom: 30%;
		background-color: #eee;
		overflow: hidden;
	}
	.carousel-items {
		width: 400%;
		height: 100%;
		position: absolute;
		left: 0;
		top: 0;
		z-index: 1;
		transition: left .5s ease-out;
		-o-transition: left .5s ease-out;
		-ms-transition: left .5s ease-out;
		-moz-transition: left .5s ease-out;
		-webkit-transition: left .5s ease-out;
	}
	.carousel-item {
		width: 25%;
		height: 100%;
		background-color: white;
		display: flex;
		justify-content: center;
		align-items: center;
	}
	.carousel-labels {
		position: absolute;
		z-index: 2;
		left: 0;
		bottom: 0;
		height: 20px;
		width: 100%;
		padding: 10px 0;
		text-align: center;
	}
	.carousel-selector {
		display: none;
	}
	.carousel-label {
		display: inline-block;
	}
	.carousel-label label {
		width: 20px;
		height: 20px;
		border-radius: 10px;
		background-color: rgba(102, 255, 102, 0.8);;
		display: block;
		cursor: pointer;
	}

	#slide1:checked ~ .carousel-items {
		left: 0;
	}
	#slide1:checked ~ .carousel-labels .carousel-label:nth-child(1) label {
		background-color: black;
	}
	#slide2:checked ~ .carousel-items {
		left: -100%;
	}
	#slide2:checked ~ .carousel-labels .carousel-label:nth-child(2) label {
		background-color: black;
	}
	#slide3:checked ~ .carousel-items {
		left: -200%;
	}
	#slide3:checked ~ .carousel-labels .carousel-label:nth-child(3) label {
		background-color: black;
	}
	#slide4:checked ~ .carousel-items {
		left: -300%;
	}
	#slide4:checked ~ .carousel-labels .carousel-label:nth-child(4) label {
		background-color: black;
	}
</style>
<script type="text/javascript">
	import axios from 'axios';
	export default{
		data(){
			return{
				auth: true,
				act: 0,
				app: [],
				ws: {},
				driver: [],
				app_status: [],
				img: {
					open: false,
					srcs: []
				},
				new_app: {
					open: false,
					adress: "",
					area: 0
				}
			}
		},
		methods:{
			sendApp(){
				console.log(this.new_app);
				axios
					.post('http://aida.market:8000/new/app', {adress: this.new_app.adress, area: this.new_app.area})
					.then(response => {
						this.new_app.open = false;
					})
					.catch(error => {
						this.new_app.open = false;
						alert(error);
					});
			},
			openImages(item){
				this.img.open = true;
				this.img.srcs = [];
				this.img.srcs.push(item.udo_side1);
				this.img.srcs.push(item.udo_side2);
				this.img.srcs.push(item.prava_side1);
				this.img.srcs.push(item.prava_side2);
			},
			async accept(item, bol){
				if(bol){
					item.acceptance = 1;
					var query = await axios.post('http://aida.market:8000/driver/accept', {driver: item});
					if(query.status==200){

					} else {
						alert(query.status);
					}
				} else {
					var query = await axios.post('http://aida.market:8000/driver/accept', {driver: item});
					if(query.status==200){

					} else {
						alert(query.status);
					}
				}
			},
			timeParse(str){
				if(str==null){
					return ""
				} else {
					var result = "";
					for(var i=0; i<str.length; i++){
						if(str[i]=="T"){
							result = result + " ";
						} else if(str[i]=="."){
							break
						} else {
							result = result + str[i];
						}
					}
					return result
				}
			},
			getDriverName(item){
				if(typeof item==null){
					return ""
				} else {
					for(var i=0; i<this.driver.length; i++){
						if(this.driver[i].id==item){
							return this.driver[i].name
						}
					}
				}
			},
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
					.post('http://aida.market:8000/get/inf', {token: this.$cookies.get('token')})
					.then(response => {
						var data = response.data;
						console.log(data);
						for(var i=0; i<data.app.length; i++){
							this.app.unshift(data.app[i]);
						}
						this.driver = data.driver;
						this.app_status = data.app_status;
						console.log(data.app_status[0].name);
					})
					.catch(error => {
						console.log(error);
					});
			},
			wss(){
				var self = this;
				var socket = new WebSocket("ws://aida.market:8001");
				socket.onmessage = function(event){
					var data = JSON.parse(event.data);
					var url = data.action;
					console.log(url);
					console.log(data.data);
					switch(url){
						case 'app':
							self.app = [];
							for(var i=0; i<data.data.length; i++){
								self.app.unshift(data.data[i]);								
							}
							break;
						case 'driver':
							self.driver = [];
							for(var i=0; i<data.data.length; i++){
								self.driver.unshift(data.data[i]);								
							}
							break;
					}
				}
				this.ws = socket;
			},
			sendToDriver(){
				axios
					.post('http://aida.market:8000/update/app/sent', {
						app: this.modal_data,
						telegram_id: this.telegram_id
					})
					.then(response => {
						this.modal = false;
					})
					.catch(error => {
						alert(error);
						this.modal = false;
					});
			}
		},
		mounted(){
			this.check();
			this.getData();
			this.wss();
		}
	}
</script>