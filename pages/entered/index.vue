<template>
	<div class="back">
		<div class="add_app" @click="new_app.open=true">
			new
		</div>
		<div class="app_inf" v-if="new_app.open">
			<form class="new_app" v-on:submit.prevent="newApp">
				<label style="margin-top: 10px;"><h3>Форма заявки</h3></label>
				<label style="margin-top: 10px;">Имя клиента</label>
				<input type="text" v-model="new_app.inf.name" style="margin-top: 10px;" required>
				<label style="margin-top: 10px;">Телефон</label>
				<input type="text" v-model="new_app.inf.phone" style="margin-top: 10px;" required>
				<label style="margin-top: 10px;">Адрес</label>
				<input type="text" v-model="new_app.inf.adress" style="margin-top: 10px;" required>
				<select v-model="new_app.inf.area" style="margin-top: 10px;" required>
					<option value="1">Город</option>
					<option value="2">Загород</option>
				</select>
				<input type="submit" style="margin-top: 10px; height: 5vh; margin-bottom: 20px">
				<a href="#" class="ai_close" style="bottom: 0; margin-left: auto; margin-right: auto; right: inherit;"@click="new_app.open=false">Закрыть</a>
			</form>
		</div>
		<div class="app_inf" v-if="app_inf.open">
			<div class="ai_main">
				<div class="ai_header">
					<h2>Заявка №{{ app_inf.inf.id }}</h2>
					<a href="#" class="ai_close" @click="app_inf.open=false">Закрыть</a>
				</div>
				<div class="ai_body">
					<div class="aib_block">
						<h3 style="margin-top: 5px">Клиент</h3>
						{{ app_inf.inf.name }}
						<div style="width: 50%; height: 5%; border-bottom-style: solid; border-color: rgba(102, 255, 102, 0.8); border-width: 0.5px"></div>
						<h3 style="margin-top: 5px">Телефон</h3>
						{{ app_inf.inf.phone }}
						<div style="width: 50%; height: 5%; border-bottom-style: solid; border-color: rgba(102, 255, 102, 0.8); border-width: 0.5px"></div>
						<h3 style="margin-top: 5px">Адрес</h3>
						{{ app_inf.inf.adress }}
						<div style="width: 50%; height: 5%; border-bottom-style: solid; border-color: rgba(102, 255, 102, 0.8); border-width: 0.5px"></div>
						<h3 style="margin-top: 5px">Время поступления заявки</h3>
						{{ timeParse(app_inf.inf.app_cometime) }}
						<div style="width: 50%; height: 5%; border-bottom-style: solid; border-color: rgba(102, 255, 102, 0.8); border-width: 0.5px"></div>
					</div>
					<div class="aib_block">
						<h3 style="margin-top: 5px" v-if="app_inf.inf.driver!=null">Водитель</h3>
						{{ getDriverName(app_inf.inf.driver) }}
						<div style="width: 50%; height: 5%; border-bottom-style: solid; border-color: rgba(102, 255, 102, 0.8); border-width: 0.5px"></div>
						<h3 style="margin-top: 5px" v-if="app_inf.inf.app_start!=null">Время начала выполнения заявки</h3>
						{{ timeParse(app_inf.inf.app_start) }}
						<div style="width: 50%; height: 5%; border-bottom-style: solid; border-color: rgba(102, 255, 102, 0.8); border-width: 0.5px"></div>
						<h3 style="margin-top: 5px" v-if="app_inf.inf.app_finish!=null">Время завершения заявки</h3>
						{{ timeParse(app_inf.inf.app_finish) }}
						<div style="width: 50%; height: 5%; border-bottom-style: solid; border-color: rgba(102, 255, 102, 0.8); border-width: 0.5px"></div>
						<h3 style="margin-top: 5px">Статус</h3>
						{{ app_status[parseInt(app_inf.inf.status)-1].name }}
						<div style="width: 50%; height: 5%; border-bottom-style: solid; border-color: rgba(102, 255, 102, 0.8); border-width: 0.5px"></div>
					</div>
					<div class="aib_block">
						<h3 style="margin-top: 5px">Тариф</h3>
						<div v-if="app_inf.inf.area==1">Город</div>
						<div v-else>Загород</div>
						<div style="width: 50%; height: 5%; border-bottom-style: solid; border-color: rgba(102, 255, 102, 0.8); border-width: 0.5px"></div>
						<h3 style="margin-top: 5px" v-if="app_inf.inf.amount!=null">Итоговая стоимость</h3>
						{{ cost(app_inf.inf.amount, app_inf.inf.driver_amount) }}
						<div style="width: 50%; height: 5%; border-bottom-style: solid; border-color: rgba(102, 255, 102, 0.8); border-width: 0.5px"></div>
						<h3 style="margin-top: 5px" v-if="app_inf.inf.amount!=null">Комиссия</h3>
						{{ app_inf.inf.amount }}
						<div style="width: 50%; height: 5%; border-bottom-style: solid; border-color: rgba(102, 255, 102, 0.8); border-width: 0.5px"></div>
					</div>
				</div>
				<div class="ai_header">
					<div class="app_accept" @click="sendApp(app_inf.inf)" v-if="app_inf.inf.status==1 || app_inf.inf.status==2">Отправить</div>
					<div class="app_accept app_dec" @click="app_inf.open=false">Отклонить</div>
				</div>
			</div>
		</div>
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
			<nuxt-link class="logo" to="/"><h1>YouDriver</h1></nuxt-link>
			<form class="no_access">
				<label id="auth">Вы не авторизированы</label>
				<nuxt-link to="/login" class="submit">Перейти на страницу авторизации</nuxt-link>
			</form>
		</div>
		<div class="main" v-else >
			<div class="left">
				<div class="left-box" style="min-height: 15vh; align-items: center; justify-content: center">
					<nuxt-link class="logo" to="/" style="margin-bottom: 0px"><h1>YouDriver</h1></nuxt-link>
				</div>
				<div class="left-box link" v-bind:class="{act: act==0}" @click="act=0">
					Заявки 
					<div v-if="popUpApp()==0" style="float: left">></div>
					<div v-else style="float: left; color: white; background-color: rgba(255, 77, 77, 0.8); padding: 1px 10px; border-radius: 50%; margin-left: 5%">{{ popUpApp() }}</div>
				</div>
				<div class="left-box link" v-bind:class="{act: act==2}" @click="act=2">
					Водители
					<div v-if="popUpDriver()==0" style="float: left">></div>
					<div v-else style="float: left; color: white; background-color: rgba(255, 77, 77, 0.8); padding: 1px 10px; border-radius: 50%; margin-left: 5%">{{ popUpDriver() }}</div>
				</div>
				<div class="left-box link exit" @click="logout()">
					Выход
				</div>
			</div>
			<div class="right">
				<div class="tables" v-if="act===0">
					<div class="row header">
						<div class="col">
							Имя
						</div>
						<div class="col">
							Телефон
						</div>
						<div class="col">
							Адрес
						</div>
						<div class="col">
							Водитель
						</div>
						<div class="col">
							Статус
						</div>
					</div>
					<div class="row" v-for="item in app" v-bind:class="{new_app: item.status==1}" @click="app_inf.open=true; app_inf.inf=item">
						<div class="col">
							{{ item.name }}
						</div>
						<div class="col">
							{{ item.phone }}
						</div>
						<div class="col">
							{{ item.adress }}
						</div>
						<div class="col">
							{{ getDriverName(item.driver) }}
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
						<div class="col" v-if="balance.open" @click="balance.open=false; balance.amount=item.balance; balance.id=item.id" >
							{{ item.balance }}
						</div>
						<div class="col" v-else>
							<form v-on:submit.prevent="changeBalance">
								<input type="text" v-model="balance.amount" style="text-align: center;">
							</form>
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
<script type="text/javascript">
	import axios from 'axios';
	export default{
		head: {
			link: [
      			{ rel: 'stylesheet', href: '/entered.css' }
    		]
		},
		data(){
			return{
				balance: {
					open: true,
					amount: 0,
					id: 0
				},
				auth: true, //валидность пользователя
				act: 0, //переключение таблиц
				ws: {}, //вебсокет
				app: [], //заявки
				driver: [], //водители
				app_status: [], //статусы заявок
				img: { //документы водителей
					open: false,
					srcs: []
				},
				app_inf: { //расширенная информация о заявке
					open: false,
					inf: {}
				},
				new_app: {
					open: false,
					inf: {}
				}
			}
		},
		methods:{
			async changeBalance(){
				var query = await axios.post('http://aida.market:8000/update/driver/balance', 
										{id: this.balance.id, balance: this.balance.amount});
				if(query.status == 200){
					this.balance.open=true;
				} else {
					alert(query.status)
				}
			},
			//маркер новой заявки
			popUpApp(){
				var result = 0;
				for(var i=0; i<this.app.length; i++){
					if(this.app[i].status==1){
						result++;
					}
				}
				return result;
			},
			//маркер нового водителя
			popUpDriver(){
				var result = 0;
				for(var i=0; i<this.driver.length; i++){
					if(this.driver[i].acceptance==0){
						result++;
					}
				}
				return result;
			},
			//просмотр документов
			openImages(item){
				this.img.open = true;
				this.img.srcs = [];
				this.img.srcs.push(item.udo_side1);
				this.img.srcs.push(item.udo_side2);
				this.img.srcs.push(item.prava_side1);
				this.img.srcs.push(item.prava_side2);
			},
			async newApp(){
				this.new_app.open=false;
				var query = await axios.post('http://aida.market:8000/new/app', this.new_app.inf);
				if(query.status!=200){
					alert(query.status);
				}
			},
			async sendApp(item){
				this.app_inf.open=false;
				var query = await axios.post('http://aida.market:8000/send/app', {app: item});
				if(query.status!=200){
					alert(query.status);
				}
			},
			//принятие или отклонение водителей
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
			//приведение времени в читабельный вид
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
			cost(a, b){
				var c = a + b;
				if(c==0){
					return ''
				} else {
					return c
				}
			},
			//считывание водителей
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
			//проверка пользователя
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
			//выход из системы
			logout(){
				this.$cookies.set('token', '');
				this.$router.push('/');
			},
			//получение первичных данных
			getData(){
				axios
					.post('http://aida.market:8000/get/inf', {token: this.$cookies.get('token')})
					.then(response => {
						var data = response.data;
						for(var i=0; i<data.app.length; i++){
							this.app.unshift(data.app[i]);
						}
						this.driver = data.driver;
						this.app_status = data.app_status;
					})
					.catch(error => {
						alert(error);
					});
			},
			//вебсокет
			wss(){
				var self = this;
				var socket = new WebSocket("ws://aida.market:8001");
				socket.onmessage = function(event){
					var data = JSON.parse(event.data);
					var url = data.action;
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
						case 'new_app':
							var app = data.data;
							var check = true;
							for(var i=0; i<self.app.length; i++){
								if(self.app[i].id == app.id){
									self.app.splice(i, 1);
									self.app.unshift(app);
									check = false;
									break;
								} else {
									check = true;
								}
							}
							if(check){
								self.app.unshift(app);
							}
							var audio = new Audio();
							audio.src='/message.mp3';
							audio.play();
							break;
						case 'new_driver':
							var driver = data.data;
							var check = true;
							for(var i=0; i<self.driver.length; i++){
								if(self.driver[i].id == driver.id){
									self.driver.splice(i, 1);
									self.driver.unshift(driver);
									check = false;
									break;
								} else {
									check = true;
								}
							}
							if(check){
								self.driver.unshift(driver);
							}
							var audio = new Audio();
							audio.src='/message.mp3';
							audio.play();
							break;
					}
				}
			}
		},
		mounted(){
			this.check();
			this.getData();
			this.wss();
		}
	}
</script>