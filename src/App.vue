<template>
   <div class="container">
    <div class="row">
      <div class="col-xs-2">
        <button @click="addBook(1,'Harry Potter and the Philosophers Stone', 100.00)">H1</button>
        <button @click="addBook(2,'Harry Potter and the Chamber of Secrets', 100.00)">H2</button>
        <button @click="addBook(3,'Harry Potter and the Prisoner of Azkaban', 100.00)">H3</button>
        <button @click="addBook(4,'Harry Potter and the Goblet of Fire', 100.00)">H4</button>
        <button @click="addBook(5,'Harry Potter and the Order of the Phoenix', 100.00)">H5</button>
        <button @click="addBook(6,'Harry Potter and the Half-Blood Prince', 100.00)">H6</button>
        <button @click="addBook(7,'Harry Potter and the Deathly Hallows', 100.00)">H7</button>
      </div>
       <div class="col-xs-6">
        <div v-for="show in storeList">
          {{show.id}} : {{show.name}} {{show.price}} x 
          <button @click="addBook(show.id, show.name, show.price)">^</button> {{show.amount}} <button @click="subBook(show.id, show.name, show.price)">v</button> 
          	= {{show.price * show.amount}}
      	   <button @click="delStore(show.id)">X</button>
          <hr>
        </div>
      </div>
      <div class="col-xs-4">
          จ่ายเงินทั้งหมด :     {{ totalPrice() }}   <br>
          ส่วนลด :        {{ discount() }}         <br>
          ชำระเงิน :     {{ totalPrice() - discount() }}
      </div>
    </div>
   </div>
</template>

<script>
export default {
  components: {
  },
  data () {
    return {
      storeList: []
    }
  },
  methods: {
    addBook: function (id, name, price) {
      var index = this.storeList.findIndex(item => item.id === id)
      if (index >= 0 && index <= 6) {
        this.storeList[index].amount += 1
      } else {
        var dataShow = {
          id: id,
          name: name,
          price: price,
          amount: 1
        }
        this.storeList.push(dataShow)
      }
    },
    totalPrice: function () {
      return (this.storeList.map(item => item.amount).reduce((sum, count) => sum + count, 0)) * 100
    },
    discount: function () {
      var discount = 0
      var items = this.storeList.map(item => {
        return {
          amount: item.amount,
          price: item.price
        }
      })
      console.log(items)
      while (items.length > 1) {
        items = items.filter(item => item.amount !== 0)
        var sumPrice = items.reduce((sum, item) => sum + item.price, 0)
        discount += ((items.length - 1) / 10) * sumPrice
        items = items.map(item => {
          return {
            amount: item.amount - 1,
            price: item.price
          }
        })
        items = items.filter(item => item.amount !== 0)
      }
      return discount
    },
    delStore: function (id) {
      console.log(id)
      var index = this.storeList.findIndex(item => item.id === id)
      this.storeList.splice(index, 1)
    },
    subBook: function (id, name, price) {
      console.log(id, name, price)
      var index = this.storeList.findIndex(item => item.id === id)
      this.storeList[index].amount -= 1
    }
  }
}
</script>

<style>
</style>
