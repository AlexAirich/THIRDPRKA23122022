<template>
  <h1 @click="BackToCatalog()" style="text-align:center;cursor: pointer;">WildberryS</h1>
  <button @click="openCart()" class="marginCart">Корзина {{ counter }}</button>
  <div class="HeadMain">
  <div class="head">
  
  <input class="marginall" v-model="searchTerm" type="text" />
  <button class="margin addbtn" @click="search">Найти </button>
  
  <select class="margin" v-model="selectedFilter" @change="filter">
    <option value="all">Все категории</option>
    <option value="Кружки для сублимации">Кружки для сублимации</option>
    <option value="Фотокамни для сублимации">Фотокамни для сублимации</option>
    <option value="Термотрансферные пленки Flex">Термотрансферные пленки Flex</option>
    <option value="Термотрансферная бумага">Термотрансферная бумага</option>
  </select>
  <button class="margin reset" @click="resetFilters">Сбросить</button>
  
</div>
</div>
 <div id="catalog">
  <div class="catalogsIMG">
  <div class="catalogs" v-for="product in products" :key="product.id">
  <ul>
    <li>
      <img :src="product.image" :alt="product.name" />
      <h2 class="center">{{ product.name }}</h2>
      <p class="center">{{ product.price }} тнг</p>
      <p class="center">Категория: {{ product.category }}</p>
      <button class="cart addbtn" @click="addToCart(product)" >Добавить в корзину</button>
      <button class="Showbtn" @click="viewDetails(product)">Посмотреть</button>
    </li>
  </ul>
</div>
</div>
</div>

<div class="detail" id="details" v-if="details" hidden="false">
  
  <img v-bind:src="details.image" v-bind:alt="details.name" />
  <div class="txt">
  <h1 class="text">{{ details.name }}</h1>
  <p class="text">Описание: {{ details.description }}</p>
  <p class="text">Цена: {{ details.price }} тнг</p>
  <p class="text">Дата публикации: {{ details.publicationDate }}</p>
  <button class="marginTop addbtn" @click="addToCart(details)">Добавить в корзину</button>
  <button class="marginTop Showbtn" @click="BackToCatalog()">Назад</button>
</div>
</div>

 <div class="CartContainer" id="cart" hidden>
   	   <div class="Header">
   	   	<h3 class="Heading">Корзина</h3>
   	   </div>

   	   <div class="Cart-Items" v-for="(item, index) in carts" :key="index">
   	   	  <div class="image-box">
   	   	  	<img :src="item.image" alt="item.name" style="height: 120px; width: 90px;">
   	   	  </div>
   	   	  <div class="about">
   	   	  	<h1 class="title">{{ item.name }}</h1>
   	   	  </div>
   	   	  <div class="counter">
          <input type="number" v-model.number="item.quantity" @input="updateTotal()" min="0">
   	   	  </div>
   	   	  <div class="prices">
   	   	  	<div class="amount">{{ item.price }} тнг</div>
   	   	  	<div class="save"><u @click="viewDetails(item)">Посмотреть</u></div>
   	   	  	<div class="remove"><u @click="removeFromCarts(index)">Удалить</u></div>
   	   	  </div>
   	   </div>
   	 <div class="checkout">
   	 <div class="total">
   	 	<div class="total-amount">Итого:{{ total }} Тенге</div>
   	 </div>
      <form action="https://kaspi.kz/webpay/partnership" target="_blank">
      <button class="addbtn">Оформить заказ</button>
      </form>
   	 <button @click="BackToCatalog()" class="button"> На главную </button>
      
      </div>
   </div>



</template>

<script>


export default {
name: 'ProductCatalog',
data() {
return {
  total:0,
  counter: 0,
  allProducts: [
    {
      id: 1,
      name: 'Кружка белая под сублимацию',
      image: 'https://images.satu.kz/50620394_w640_h640_50620394.jpg',
      price: 540,
      category: 'Кружки для сублимации',
      description: 'Классическая белая кружка в стандартном исполнении 11OZ  самый популярный подарочный  атрибут! Идеально подходящий под кружечный термопресс. Прямая поставка с завода производителя гарантирует  отличную цену! Покупая кружки в компании Авторское будьте уверенны в отсутствии бракованных и битых кружек. Поставляем кружки для сублимации только с качественным покрытием с отличной белизной керамики высокого класса. Мы имеем сертификаты соответствия для спокойствия клиента и Вашего бизнеса.',
      publicationDate: '2023-01-01',
      quantity: 1,
    },
    {
      id: 2,
      name: 'Кружка "Хамелеон" черный цвет (матовые)',
      image: 'https://images.satu.kz/5936001_w640_h640_kruzhka-hameleon-chernyj.jpg',
      price: 710,
      category: 'Кружки для сублимации',
      description: 'Кружка под сублимацию "Хамелеон" Черный цвет изготовлена из керамики с полимерным покрытием, используется для нанесения изображения методом сублимационного переноса.',
      publicationDate: '2023-01-02',
      quantity: 1,
    },
    {
      id: 3,
      name: 'Кружка матовая пивная кружка с градиентом красного цвета',
      image: 'https://images.satu.kz/163436777_w640_h640_kruzhka-matovaya-pivnaya.jpg',
      price: 1600,
      category: 'Кружки для сублимации',
      description: 'Кружка матовая пивная кружка  с градиентом красного цвета с полимерным покрытием, используется для нанесения изображения методом сублимационного переноса.',
      publicationDate: '2023-01-03',
      quantity: 1,
    },
    {
      id: 4,
      name: 'Фото камень "Прямоугольный" с подставкой',
      image: 'https://images.satu.kz/2855922_w640_h640_foto-kamen-pryamougolnyj.jpg',
      price: 1680,
      category: 'Фотокамни для сублимации',
      description: 'Фото камень 15*20 см глянец поверхность',
      publicationDate: '2023-01-04',
      quantity: 1,
    },
    {
      id: 5,
      name: 'Фото камень "Скошеный угол" с подставкой',
      image: 'https://images.satu.kz/2855953_w640_h640_foto-kamen-skoshenyj.jpg',
      price: 1100,
      category: 'Фотокамни для сублимации',
      description: 'Фото камень 20*20 см',
      publicationDate: '2023-01-05',
      quantity: 1,
    },
    {
      id: 6,
      name: 'Термоклеевая пленка для фиксации',
      image: 'https://images.satu.kz/70694155_w640_h640_termokleevaya-plenka-dlya.jpg',
      price: 750,
      category: 'Термотрансферные пленки Flex',
      description: 'Термоклеевая пленка предназначена для термического приклеивания нашивки, шеврона, патча, к любому изделию (бейсболка, футболка, жакеты, джинсы, итд) , вместо пришивания.',
      publicationDate: '2023-01-06',
      quantity: 1,
    },
    {
      id: 7,
      name: 'БЕЛАЯ Термотрансферная пленка (ФЛЕКС)',
      image: 'https://images.satu.kz/142454187_w640_h640_belaya-termotransfernaya-plenka.jpg',
      price: 2400,
      category: 'Термотрансферные пленки Flex',
      description: 'Виниловые пленки для термотрансфера на ткани (PU Flex)',
      publicationDate: '2023-01-07',
      quantity: 1,
    },
    {
      id: 8,
      name: 'СИНЯЯ Термотрансферная пленка (ФЛЕКС)',
      image: 'https://images.satu.kz/142454405_w640_h640_sinyaya-termotransfernaya-plenka.jpg',
      price: 10000,
      category: 'Термотрансферные пленки Flex',
      description: 'Виниловые пленки для термотрансфера на ткани (PU Flex)',
      publicationDate: '2023-01-08',
      quantity: 1,
    },
    {
      id: 9,
      name: 'Сублимационная бумага универсальная односторонняя A3',
      image: 'https://images.satu.kz/61822693_w640_h640_sublimatsionnaya-bumaga-universalnaya.jpg',
      price: 3700,
      category: 'Термотрансферная бумага',
      description: 'Этот промежуточный носитель чернил, идеальный тип бумаги со специальным покрытием, созданный для качественного сублимационного переноса изображения на различные предметы,такие как; Керамические кружки, тарелки,футболки,текстильные изделия из полиэстера, двухслойные футболки,таблички из металла, и все другие изделия предназначенные для сублимационной печати.',
      publicationDate: '2023-01-09',
      quantity: 1,
    },
    {
      id: 10,
      name: 'Бумага трансферная для темных тканей A4 (Super quality)',
      image: 'https://images.satu.kz/3196857_w640_h640_bumaga-transfernaya-dlya.jpg',
      price: 525,
      category: 'Термотрансферная бумага',
      description: 'Этот промежуточный носитель чернил, идеальный тип бумаги со специальным покрытием, созданный для качественного сублимационного переноса изображения на различные предметы,такие как; Керамические кружки, тарелки,футболки,текстильные изделия из полиэстера, двухслойные футболки,таблички из металла, и все другие изделия предназначенные для сублимационной печати.',
      publicationDate: '2023-01-10',
      quantity: 1,
    },
    {
      id: 11,
      name: 'Бумага трансферная для светлых тканей A4 (Super quality)',
      image: 'https://images.satu.kz/3196844_w640_h640_bumaga-transfernaya-dlya.jpg',
      price: 330,
      category: 'Термотрансферная бумага',
      description: 'Этот промежуточный носитель чернил, идеальный тип бумаги со специальным покрытием, созданный для качественного сублимационного переноса изображения на различные предметы,такие как; Керамические кружки, тарелки,футболки,текстильные изделия из полиэстера, двухслойные футболки,таблички из металла, и все другие изделия предназначенные для сублимационной печати.',
      publicationDate: '2023-01-10',
      quantity: 1,
    },
    

  ],
  searchTerm: '',
  selectedFilter: 'all',
  products: [
      {
      id: 1,
      name: 'Кружка белая под сублимацию',
      image: 'https://images.satu.kz/50620394_w640_h640_50620394.jpg',
      price: 540,
      category: 'Кружки для сублимации',
      description: 'Классическая белая кружка в стандартном исполнении 11OZ  самый популярный подарочный  атрибут! Идеально подходящий под кружечный термопресс. Прямая поставка с завода производителя гарантирует  отличную цену! Покупая кружки в компании Авторское будьте уверенны в отсутствии бракованных и битых кружек. Поставляем кружки для сублимации только с качественным покрытием с отличной белизной керамики высокого класса. Мы имеем сертификаты соответствия для спокойствия клиента и Вашего бизнеса.',
      publicationDate: '2023-01-01',
      quantity: 1,
    },
    {
      id: 2,
      name: 'Кружка "Хамелеон" черный цвет (матовые)',
      image: 'https://images.satu.kz/5936001_w640_h640_kruzhka-hameleon-chernyj.jpg',
      price: 710,
      category: 'Кружки для сублимации',
      description: 'Кружка под сублимацию "Хамелеон" Черный цвет изготовлена из керамики с полимерным покрытием, используется для нанесения изображения методом сублимационного переноса.',
      publicationDate: '2023-01-02',
      quantity: 1,
    },
    {
      id: 3,
      name: 'Кружка матовая пивная кружка с градиентом красного цвета',
      image: 'https://images.satu.kz/163436777_w640_h640_kruzhka-matovaya-pivnaya.jpg',
      price: 1600,
      category: 'Кружки для сублимации',
      description: 'Кружка матовая пивная кружка  с градиентом красного цвета с полимерным покрытием, используется для нанесения изображения методом сублимационного переноса.',
      publicationDate: '2023-01-03',
      quantity: 1,
    },
    {
      id: 4,
      name: 'Фото камень "Прямоугольный" с подставкой',
      image: 'https://images.satu.kz/2855922_w640_h640_foto-kamen-pryamougolnyj.jpg',
      price: 1680,
      category: 'Фотокамни для сублимации',
      description: 'Фото камень 15*20 см глянец поверхность',
      publicationDate: '2023-01-04',
      quantity: 1,
    },
    {
      id: 5,
      name: 'Фото камень "Скошеный угол" с подставкой',
      image: 'https://images.satu.kz/2855953_w640_h640_foto-kamen-skoshenyj.jpg',
      price: 1100,
      category: 'Фотокамни для сублимации',
      description: 'Фото камень 20*20 см',
      publicationDate: '2023-01-05',
      quantity: 1,
    },
    {
      id: 6,
      name: 'Термоклеевая пленка для фиксации',
      image: 'https://images.satu.kz/70694155_w640_h640_termokleevaya-plenka-dlya.jpg',
      price: 750,
      category: 'Термотрансферные пленки Flex',
      description: 'Термоклеевая пленка предназначена для термического приклеивания нашивки, шеврона, патча, к любому изделию (бейсболка, футболка, жакеты, джинсы, итд) , вместо пришивания.',
      publicationDate: '2023-01-06',
      quantity: 1,
    },
    {
      id: 7,
      name: 'БЕЛАЯ Термотрансферная пленка (ФЛЕКС)',
      image: 'https://images.satu.kz/142454187_w640_h640_belaya-termotransfernaya-plenka.jpg',
      price: 2400,
      category: 'Термотрансферные пленки Flex',
      description: 'Виниловые пленки для термотрансфера на ткани (PU Flex)',
      publicationDate: '2023-01-07',
      quantity: 1,
    },
    {
      id: 8,
      name: 'СИНЯЯ Термотрансферная пленка (ФЛЕКС)',
      image: 'https://images.satu.kz/142454405_w640_h640_sinyaya-termotransfernaya-plenka.jpg',
      price: 10000,
      category: 'Термотрансферные пленки Flex',
      description: 'Виниловые пленки для термотрансфера на ткани (PU Flex)',
      publicationDate: '2023-01-08',
      quantity: 1,
    },
    {
      id: 9,
      name: 'Сублимационная бумага универсальная односторонняя A3',
      image: 'https://images.satu.kz/61822693_w640_h640_sublimatsionnaya-bumaga-universalnaya.jpg',
      price: 3700,
      category: 'Термотрансферная бумага',
      description: 'Этот промежуточный носитель чернил, идеальный тип бумаги со специальным покрытием, созданный для качественного сублимационного переноса изображения на различные предметы,такие как; Керамические кружки, тарелки,футболки,текстильные изделия из полиэстера, двухслойные футболки,таблички из металла, и все другие изделия предназначенные для сублимационной печати.',
      publicationDate: '2023-01-09',
      quantity: 1,
    },
    {
      id: 10,
      name: 'Бумага трансферная для темных тканей A4 (Super quality)',
      image: 'https://images.satu.kz/3196857_w640_h640_bumaga-transfernaya-dlya.jpg',
      price: 525,
      category: 'Термотрансферная бумага',
      description: 'Этот промежуточный носитель чернил, идеальный тип бумаги со специальным покрытием, созданный для качественного сублимационного переноса изображения на различные предметы,такие как; Керамические кружки, тарелки,футболки,текстильные изделия из полиэстера, двухслойные футболки,таблички из металла, и все другие изделия предназначенные для сублимационной печати.',
      publicationDate: '2023-01-10',
      quantity: 1,
    },
    {
      id: 11,
      name: 'Бумага трансферная для светлых тканей A4 (Super quality)',
      image: 'https://images.satu.kz/3196844_w640_h640_bumaga-transfernaya-dlya.jpg',
      price: 330,
      category: 'Термотрансферная бумага',
      description: 'Этот промежуточный носитель чернил, идеальный тип бумаги со специальным покрытием, созданный для качественного сублимационного переноса изображения на различные предметы,такие как; Керамические кружки, тарелки,футболки,текстильные изделия из полиэстера, двухслойные футболки,таблички из металла, и все другие изделия предназначенные для сублимационной печати.',
      publicationDate: '2023-01-10',
      quantity: 1,
    },
    
  ] ,
  details:[],
  carts: [],
}
},
methods: {
search() {
  this.products = this.allProducts 
  this.products = this.products.filter(product =>
    product.name.toLowerCase().includes(this.searchTerm.toLowerCase())
  )
},
filter() {
  this.products = this.allProducts
  if (this.selectedFilter !== 'all') {
    this.products = this.products.filter(product =>
      product.category === this.selectedFilter
    )
  }
},
resetFilters() {
  this.searchTerm = ''
  this.selectedFilter = 'all'
  this.products = this.allProducts
},
viewDetails(product) {
  this.details = product
  document.getElementById("catalog").hidden = true
  document.getElementById("details").hidden = false
  document.getElementById("cart").hidden = true
},
BackToCatalog(){
  document.getElementById("catalog").hidden = false
  document.getElementById("details").hidden = true
  document.getElementById("cart").hidden = true

},
openCart(){
  document.getElementById("catalog").hidden = true
  document.getElementById("details").hidden = true
  document.getElementById("cart").hidden = false


},
addToCart(product){
  if (this.carts.length == 0){  
  this.counter++
  this.carts.push(product)
  this.updateTotal()
  return
  }
  if( this.carts.length > 0){
    for(let i = 0; i < this.carts.length; i++ ){
      if(this.carts[i].id === product.id){
        product.quantity++
        this.updateTotal() 
      return
      }
    }

  this.counter++
  this.carts.push(product)
  this.updateTotal()
  }
},
updateTotal() {
    this.total = this.carts.reduce((acc, item) => acc + (item.price * item.quantity), 0)
  },
  removeFromCarts(index) {
    this.carts.splice(index, 1)
    this.updateTotal()
    this.counter--
  },

}
}
</script>
<style>
*{
    margin: 0;
    padding: 0;
   }
.addbtn{
  background-color: #4caf50;
  color: white;
  padding: 14px 20px;
  margin: 1px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.addbtn:hover {
  background-color: #46994a;
}
.Showbtn{
  background-color: rgb(79, 79, 255);
  color: white;
  padding: 14px 20px;
  margin: 1px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
      margin-left: 45px;
}

.Showbtn:hover {
  background-color: rgb(52, 52, 250);
}
  img{
    height: 400px;
    width: 300px;
    border-radius: 10px;
  }
  li {
    list-style-type: none; 
   }
   .catalogsIMG{
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
   }
   .catalogs{
    width: 380px;
    height: 600px;
    margin-top: 30px;
   }
   
   .head{
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
      width: 305px;
    height: 100px;
    align-items: stretch;
    align-content: space-around;
   }
   #catalog{
    margin-top: 30px;
   }
   h1{
    margin-left: 2%;
    font-size: 45px;
    font-family: 'Play', sans-serif;
    color: rgb(79, 79, 255);
   }
   .margin{
    margin-left: 5px;
   }
   .reset{
  background-color: #c52121;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
.reset:hover{
  background-color: #8b1d1d;
}
.HeadMain{
display: flex;
justify-content: center;
}
   .marginall{
    margin-left:20px;
   }
   .center{
    font-family: 'Play', sans-serif;
    text-align: center;
    color: rgb(78, 10, 141);
   }
   .cart{
    margin-left: 45px;
    margin-top: 5px;
   }
   .details{
    margin-left: 15px;
    margin-top: 5px;
   }
   .marginCart{
    position: fixed;
    top: 17px;
    left: 90%;
    background-color: rgb(79, 79, 255);
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    width: 100px;
    height: 50px;
    font-size: 20px;
   }
   .detail{
    width: 1000px;
    height: 400px;
    margin-left: 30%;
    margin-top: 10%;
   }
   .txt{
    margin-left: 344px;
    margin-top: -370px;
   }
   .marginTop{
    margin-top: 10px;
    margin-left: 5px;
   }
   .text{
    font-family: 'Play', sans-serif;
    font-size: 20px;
   }
   #cart{
    margin-top: 50px;
    margin-left: 30%;
    width: 60%;

   }
   .border{
    border: 1px solid black;
    width: 180px;
    font-size: 20px;
    border-radius: 5px;
   }
   .del{
  width: 100%;
  background-color: #c52121;
  color: white;
  padding: 14px 20px;
  margin: 8px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
.del:hover{
  background-color: #8b1d1d;
}
.CartContainer{
	width: 70%;
	height: 90%;
	background-color: #ffffff;
    border-radius: 20px;
    box-shadow: 0px 10px 20px #1687d933;
}

.Header{
	margin: auto;
	width: 90%;
	height: 15%;
	display: flex;
	justify-content: space-between;
	align-items: center;
}

.Heading{
	font-size: 20px;
	font-family: 'Open Sans';
	font-weight: 700;
	color: #2F3841;
}

.Action{
	font-size: 14px;
	font-family: 'Open Sans';
	font-weight: 600;
	color: #E44C4C;
	cursor: pointer;
	border-bottom: 1px solid #E44C4C;
}

.Cart-Items{
	margin: auto;
	width: 90%;
	height: 30%;
	display: flex;
	justify-content: space-between;
	align-items: center;
}
.image-box{
	width: 15%;
	text-align: center;
}
.about{
	height: 100%;
	width: 24%;
}
.title{
	padding-top: 10px;
	line-height: 10px;
	font-size: 12px;
	font-family: 'Open Sans';
	font-weight: 800;
	color: #202020;
}
.subtitle{
	line-height: 10px;
	font-size: 18px;
	font-family: 'Open Sans';
	font-weight: 600;
	color: #909090;
}

.counter{
	width: 15%;
	display: flex;
	justify-content: space-between;
	align-items: center;
}
.btn{
	width: 40px;
	height: 40px;
	border-radius: 50%;
	background-color: #d9d9d9;
	display: flex;
	justify-content: center;
	align-items: center;
	font-size: 20px;
	font-family: 'Open Sans';
	font-weight: 900;
	color: #202020;
	cursor: pointer;
}
.count{
	font-size: 20px;
	font-family: 'Open Sans';
	font-weight: 600;
	color: #202020;
}

.prices{
	height: 100%;
	text-align: right;
}
.amount{
	padding-top: 20px;
	font-size: 26px;
	font-family: 'Open Sans';
	font-weight: 800;
	color: #202020;
}
.save{
	padding-top: 5px;
	font-size: 14px;
	font-family: 'Open Sans';
	font-weight: 600;
	color: #1687d9;
	cursor: pointer;
}
.remove{
	padding-top: 5px;
	font-size: 14px;
	font-family: 'Open Sans';
	font-weight: 600;
	color: #E44C4C;
	cursor: pointer;
}

.pad{
	margin-top: 5px;
}

hr{
	width: 66%;
	float: right;
	margin-right: 5%;
}
.checkout{
  float: left;
	margin-right: 5%;
	width: 28%;
}
.total{
	width: 100%;
	display: flex;
	justify-content: space-between;
}
.Subtotal{
	font-size: 22px;
	font-family: 'Open Sans';
	font-weight: 700;
	color: #202020;
}
.items{
	font-size: 16px;
	font-family: 'Open Sans';
	font-weight: 500;
	color: #909090;
	line-height: 10px;
}
.total-amount{
	font-size: 36px;
	font-family: 'Open Sans';
	font-weight: 900;
	color: #202020;
}
.button{
  background-color: rgb(79, 79, 255);
  color: white;
  padding: 14px 20px;
  margin: 1px 0;
  border: none;
  border-radius: 4px;
  cursor: pointer;
      margin-top: 15px;
      margin-left:15px;
}
.button:hover {
  background-color: rgb(52, 52, 250);
}
</style>