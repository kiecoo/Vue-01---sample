<template>
  <div class="container-fluid mt-5">
    <h1> List of {{pageTitle}}</h1>
    <table class="table table-striped">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">First</th>
          <th scope="col">Last</th>
          <th scope="col">Handle</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item , i ) in listData" :key="i" :class="{'bg-info':item.selected}">
          <!-- ERR: clickMe(item)後bg-info無法成功顯示 在 selected為ture的item ,
                    因 新屬性selected  尚未＄set -->
          <th scope="row">{{i+1}}</th>
          <td><img :src="item.picture.medium" alt=""></td>
          <td >
            <span @click="nameClicked(item.name.first)">{{item.name.first}}</span>
            <!--       wrong
             <span @click="nameClicked()">{{item.name.first}}</span> -->
          </td>
          <td>{{item.email}}</td>
          <td> <button class="btn btn-outline-primary" @click="clickMe(item)">click</button> </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      listData: []
    }
  },
  props: {
    pageTitle: {
      type: String,
      default: ''
    }
  },
  mounted () {
    this.getData()
  },
  methods: {
    getData () {
      let vm = this
      vm.axios.get('https://randomuser.me/api/?results=50')
        .then(function (response) {
          console.log(response)
          vm.listData = response.data.results
          vm.listData.forEach(element => {
            vm.$set(element, 'selected', false)
            // WRONG:  this.$set(.....) 需用vm代替this
          })
        })
        .catch(function (error) {
          console.log(error)
        })
    },
    nameClicked (person) {
      this.$emit('sayHi', person)
      console.log(`${person}`)
    },
    clickMe (item) {
      item.selected = !item.selected
      console.log(item.selected)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/* h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
} */
</style>
