<template>
    <div>

      <div id="title">
        <h5>ОПТОВЫЙ СКЛАД</h5>
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
            </thead>
            <tbody class="text-left ">
            <tr v-for="product in products" v-bind:key="product.id">
              <td>{{product.id}}</td>
              <td>{{product.model}}</td>
              <td>{{product.color}}</td>
              <td>{{product.size}}</td>
              <td>{{product.quantity}}</td>
              <td>{{product.price}}</td>
            </tr>
            </tbody>
          </table>
        </div>
        <form id="form">
          <p>перемещение товара</p>
          <p><input type="text" v-model="ProductTraffic.model" name="model" placeholder="введите номер модели"></p>
          <p><input type="text" v-model="ProductTraffic.color" name="color" placeholder="введите цвет"></p>
          <p><input type="text" v-model="ProductTraffic.size" name="size" placeholder="введите размер"></p>
          <p><input type="text" v-model="ProductTraffic.quantity" name="quantity" placeholder="введите количество"></p>
          <p><input type="text" v-model="ProductTraffic.price" name="price" placeholder="введите цену"></p>
          <button class="alert-danger" type="button" v-on:click="traffic">переместить товар</button>
        </form>
        <form id="search">
          <p>поиск товара на оптовом складе</p>
          <p><input type="text" v-model="search" name="name" placeholder="введите номер модели"></p>
          <button type="button" v-on:click="searchProduct">найти</button>
          <button class="alert-danger" type="button" v-on:click="searchDelete">обнулить поиск</button>
        </form>
        <div id="resSearch">
          <p>результаты поиска :</p>
          <ol v-for="elem in res">
            <li>модель{{' : '}}{{elem.model}}</li>
            <li>цвет{{' : '}}{{elem.color}}</li>
            <li>размер{{' : '}}{{elem.size}}</li>
            <li>количество{{' : '}}{{elem.quantity}}</li>
            <li>цена{{' : '}}{{elem.price}}</li>

          </ol>
      </div>
    </div>
    </div>
</template>

<script>


    export default {
        name: "component2",
        data() {
            return {
              products: [
                {id: 1, model: '1', color: '1', size: '1',quantity: 50, price: '1'},
                {id: 2, model: '2', color: 'green',size: 'xl', quantity: 48, price: '35'},
                {id: 3, model: '3', color: 'yellow',size: 'xl', quantity: 49, price: '40'},
                {id: 4, model: '4', color: 'black',size: 'xl', quantity: 46, price: '45'},
                {id: 5, model: '5', color: ' red',size: 'xl', quantity: 53, price: '50'},
                {id: 6, model: '9', color: '9',size: '9', quantity: 53, price: '9'},
                {id: 7, model: '7', color: '7',size: '7', quantity: 53, price: '7'},

              ],
              ProductTraffic:{
                 id: '',
                 model: '',
                 color: '',
                 size: '',
                 quantity:'',
               },
              search: [],
              res: [],
              products2:[],
            }

        },
        methods: {
          searchProduct: function () {
            let res1 = this.products.filter(elem => this.search.includes(elem.model) );

            // с includes работает с this.search.model===(elem.model) нет почему???

            // let res1 = this.products.filter(elem => this.search.model===elem.model );
            if (res1.length == 0) {
              alert('совпадений не найдено');
            }
            this.res = res1;
            this.search = [];
          },
          searchDelete: function () {
            this.res = [];
          },
          traffic:function () {
            debugger;
            let res2 = this.products.filter(elem => this.ProductTraffic.model===elem.model   );
            // let res2 = this.products.filter(elem => this.ProductTraffic.model.includes(elem.model) && this.ProductTraffic.color.includes(elem.color) && this.ProductTraffic.size.includes(elem.size)  );
            if ( res2==''){
              alert('модели с такими характеристиками на складе нет');
              this.ProductTraffic={};
              return
            }
            else { this.products2=res2[0];
              console.dir( this.products2[0]);
              this.products2.quantity= this.products2.quantity-this.ProductTraffic.quantity;

              this.$emit('TrafficCom2ComPar',this.ProductTraffic);

              this.ProductTraffic={}
              // this.ProductTraffic=undefined;
            }

          }
        }

    }
</script>

<style scoped>
  p{
    color: orangered;
  }
  table, th, td {
    border: 1px solid black;
  }
  #table {
    position: absolute;
    top: 50px;
    margin: 10px;
  }
  #title{
    position: absolute;
    top: 20px;
    left: 100px;
    color: blue;
  }
  #form {
    position: absolute;
    left: 640px;
    top: 10px;
    margin: 10px;
  }
  #search {
    position: absolute;
    left: 640px;
    top: 340px;
    margin: 10px;
  }
  #resSearch {
    position: absolute;
    left: 640px;
    top: 470px;
    margin: 10px;
  }
</style>
