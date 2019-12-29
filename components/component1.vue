<template>

    <div>
        <div id="message" v-if=" hasAdded==false && com21 != undefined" >произведено перемещение <br>товара на этот<br>склад<br>нажмите кнопку<br>"ДОБАВИТЬ"
            <ol>
              <li>модель:{{com21.model}}</li>
               <li>цвет:{{com21.color}}</li>
                <li>размер:{{com21.size}}</li>
                 <li>цена:{{com21.price}}</li>
                  <li>количество:{{com21.quantity}}</li>
            </ol>
          </div>
        <div id="title">




<!--          <p v-if="com21!=undefined"> получил из компонента 2 {{this.com21.model}}</p>-->
      <h5>ОСНОВНОЙ СКЛАД</h5>
        </div>
      <div>

        <div id="table">
          <table>
            <thead class="text-xl-center">
            <th>id</th>
            <th> модель</th>
            <th>цвет</th>
            <th>размер</th>
            <th>количество</th>
            <th>цена</th>
            <td class="alert-danger" v-on:click="add">добавить</td>
            </thead>
            <tbody class="text-left ">
            <tr v-for="product in products" v-bind:key="product.id">
              <td>{{product.id}}</td>
              <td>{{product.model}}</td>
              <td>{{product.color}}</td>
              <td>{{product.size}}</td>
              <td>{{product.quantity}}</td>
              <td>{{product.price}}</td>
              <td v-on:click="deleteProduct(product)">удалить</td>
            </tr>

            </tbody>
          </table>
        </div>
      </div>

      <form id="form">
        <p>получение товара</p>
        <p><input type="text" v-model="newModel.model" name="model" placeholder="введите номер модели"></p>
        <p><input type="text" v-model="newModel.color" name="color" placeholder="введите цвет"></p>
        <p><input type="text" v-model="newModel.size" name="size" placeholder="введите размер"></p>
        <p><input type="text" v-model="newModel.quantity" name="quantity" placeholder="введите количество"></p>
        <p><input type="text" v-model="newModel.price" name="price" placeholder="введите цену"></p>
      </form>






      <form id="search">
        <p>поиск товара на основном складе</p>
        <p><input type="text" v-model="search" name="name" placeholder="введите номер модели"></p>
        <button type="button" v-on:click="searchProduct">найти</button>
        <button class="alert-danger" type="button" v-on:click="searchDelete">обнулить поиск</button>
      </form>
     <div id="resSearch">
<p>результаты поиска :</p>
      <ol v-for="elem in res">
        <!--            работает с methods-->
        <!--      <ol v-for="elem in searchUser">-->
        <!--        работает с computed-->

        <li>модель{{' : '}}{{elem.model}}</li>
        <li>цвет{{' : '}}{{elem.color}}</li>
        <li>размер{{' : '}}{{elem.size}}</li>
        <li>количество{{' : '}}{{elem.quantity}}</li>
        <li>цена{{' : '}}{{elem.price}}</li>
      </ol>

    </div>
      <div id="modal11">
        <button class="alert-danger" @click="showModal = true">реализация со склада</button>
        <modal v-if="showModal" @close="showModal = false"> </modal>
      </div>
      <div v-if='showModal == true' class="modal-container">
        <div id="sale">
        <p><input type="text" v-model=" modal.model" name="model" placeholder="введите номер модели"></p>
        <p><input type="text" v-model=" modal.color" name="color" placeholder="введите цвет"></p>
        <p><input type="text" v-model=" modal.size" name="size" placeholder="введите размер"></p>
        <p><input type="text" v-model=" modal.quantity" name="quantity" placeholder="введите количество"></p>
        <p><input type="text" v-model=" modal.price" name="price" placeholder="введите цену"></p>
        </div>
        <button  id="closewindow" class="alert-danger" @click="showModal = false">
          закрыть окно
        </button>
        <button id="btnsale" v-on:click="sale">подтвердите продажу</button>
      </div>


    </div>
</template>

<script>


export default {
        name: "component1",


  props: ['com21'],//props получаю от родителя com2


        data() {
            return {
              products: [
                {id: 1, model: '1', color: '1', size: '1',quantity: 50, price: '1'},
                {id: 2, model: '2', color: '2',size: '2', quantity: 48, price: '2'},
                {id: 3, model: '3', color: 'yellow',size: 'xl', quantity: 49, price: '40'},
                {id: 4, model: '4', color: 'black',size: 'xl', quantity: 46, price: '45'},
                {id: 5, model: '5', color: ' red',size: 'xl', quantity: 53, price: '50'},
                {id: 6, model: '6', color: 'red',size: 'xl', quantity: 53, price: '70'},

              ],
              newModel: {
                id: '',
                model: '',
                color: '',
                size: '',
                quantity:'',
                price: ''
              },
              modal: {

                model: '',
                color: '',
                size: '',
                quantity:'',
                price: ''
              },
              iHaveModel:{},
              salemodel:{},
              search: '',
              res: [],
              showModal: false,
              hasAdded: false,
            }

        },
        methods: {
          deleteProduct: function (product) {
            let id = product.id;
            this.products.splice(this.products.findIndex(v => v.id == id), 1);
          },

          add: function () {// добавление новых и уже существующих моделей
              if(this.com21!==undefined){


                this.newModel=this.com21;
                this.hasAdded = true;
                // this.com21={};
              }


          else if(this.newModel.model==undefined || this.newModel.model== ''){//если пустая строка ничего не добавляет
              alert('введите описание модели');
              return 0;
            }

            let res3=this.products.filter(elem => this.newModel.model==elem.model && this.newModel.color==elem.color && this.newModel.size==elem.size);//ищет  модель в уже существующих
            this.iHaveModel=res3[0];
            if(this.iHaveModel==undefined){     //если модели не было
              this.newModel.id = this.products.length + 1;//присваивает новый id
              this.products.push(this.newModel);//помещает новую модель в массив существующих моделей
            }


            else if(this.newModel.model==this.iHaveModel.model && this.newModel.color==this.iHaveModel.color && this.newModel.size==this.iHaveModel.size){ //если модель была
              this.newModel.id = this.iHaveModel.id;// оставляет старый id
              this.iHaveModel.quantity= Number(this.iHaveModel.quantity)+Number(this.newModel.quantity)}//добавляет новое количество к уже существующему

            else if(this.newModel.model==this.iHaveModel.model && (this.newModel.color!=this.iHaveModel.color || this.newModel.size!=this.iHaveModel.size)){ //если модель была
              this.products.push(this.newModel)}
            this.newModel = {};//очищает поле ввода
            this.products.sort(function (a, b) {
              if (a.model > b.model) {
                return 1;
              }
              if (a.model < b.model) {
                return -1;
              }
              return 0;
            })


          },
          searchProduct: function () {
            let res1 = this.products.filter(elem => this.search===(elem.model) );

            // с includes работает с this.search.model===(elem.model) нет почему???

            // let res1 = this.products.filter(elem => this.search.includes(elem.model) );
            if (res1.length == 0) {
              alert('совпадений не найдено');
            }
            this.res = res1;

            this.search = '';

          },
          searchDelete: function () {
            this.res = [];
          },
          sale:function () {
            debugger;
            if(this.modal.model=='' || this.modal.color=='' || this.modal.size=='' || this.modal.price==''){
              alert('заполните поля ввода');
              this.modal.model='';
              this.modal.color='';
              this.modal.size='';
              this.modal.quantity='';
              this.modal.price='';
              return 0
            }

            let res3=this.products.filter(elem => this.modal.model==elem.model && this.modal.color==elem.color && this.modal.size==elem.size && this.modal.price==elem.price);//ищет  модель в уже существующих
            this.salemodel=res3[0];
           // if (this.salemodel.model!=this.modal.model || this.salemodel.color!=this.modal.color || this.salemodel.size!=this.modal.size || this.salemodel.price!=this.modal.price){
            if(this.salemodel==undefined){
            alert('при заполнении полей была допущена ошибка');
             // this.modal.model='';
             // this.modal.color='';
             // this.modal.size='';
             // this.modal.quantity='';
             // this.modal.price='';
              this.modal={}
            return 0
            }
            this.salemodel.quantity=Number(this.salemodel.quantity)-Number(this.modal.quantity);
            // this.modal={};
            this.modal.model='';
            this.modal.color='';
            this.modal.size='';
            this.modal.quantity='';
            this.modal.price='';
          }
},

  computed: {
// spisanie(){
//   alert('было выполнено перемещение товара с оптового склада нажмите "ДОБАВИТЬ" ')

// }
  }
//   updated() {
// //
// //             this.$parent.$on('spisanie',function (ProductTraffic) {
// //               this.newModel=ProductTraffic;
// // add();
// //
// //             })
//
//
//   }
//   updated() {
//     if(this.com21!==null){
//       alert('было совершено перемещение товара на основной склад НАЖАТЬ "ДОБАВИТЬ"');
//
//       this.newModel=this.com21;
//       this.com21=null;
//
//     }
//
//   }

}
</script>

<style scoped>
  table, th, td {
    border: 1px solid black;
  }
  p{
    color: orangered;
  }
  #message{
     position: absolute;
    top: 20px;
    left: 430px;
    color: blue;
  }


  #title{
    position: absolute;
    top: 20px;
    left: 110px;
    color: blue;
  }
  #table {
    position: absolute;
    top: 40px;
    margin: 10px;
  }
  #form {
    position: absolute;
    left: 640px;
    top: 10px;
    margin: 10px;
  }
  .modal-container {
    position: absolute;
    top: 100px;
    left: 100px;
    width: 600px;
    height: 300px;
    opacity:1;
    background-color:whitesmoke;
    border-radius: 10px;
    border:1px solid greenyellow;
  }
  #modal11{
    position: absolute;
    left: 640px;
    top: 280px;
    margin: 10px;
  }
  #sale{
    position: absolute;
    left: 20px;
    top: 20px;
    margin: 5px;
  }
  #btnsale{
    position: absolute;
    left: 20px;
    top: 245px;
    margin: 5px;
    background:lightgreen;
  }
  #closewindow{
    position: absolute;
    left: 450px;
    top: 245px;
    margin: 5px;
    background:peachpuff;
  }
  #search {
    position: absolute;
    left: 640px;
    top: 320px;
    margin: 10px;
  }
  #resSearch {
    position: absolute;
    left: 640px;
    top: 450px;
    margin: 10px;
  }
</style>
