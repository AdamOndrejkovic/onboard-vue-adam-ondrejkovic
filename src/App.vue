<template>
  <main>
<!--    While loading show spinner-->
    <div v-if="loading" class="wrapper">
      <div class="loader"></div>
    </div>
<!--    If error occurs show an message-->
    <div v-else-if="error" class="wrapper">
      <div class="error"><p>Error occurred please try again later.</p></div>
    </div>
<!--    If data loads and no error is caught displays users-->
    <div v-else-if="!error || !loading" class="cardWrapper">
      <div class="filter"> Filter: <input v-on:keydown.esc="clearInput" type="text" placeholder="Filter Users" v-model="searchQuery"></div>
      <div class="p-3">
        <hr>
        Selected: {{selected}} <br>
        Unselected: {{unselected}}
        <hr>
      </div>
      <div class="pt-3 grid grid-cols-1 md:grid-cols-2  xl:grid-cols-4 lg:grid-cols-3 gap-3">
        <!--      Loop through data and for each user create a card-->
        <UserCard v-for="user in filteredUser" :key="user.id" @changeSelectionCounter="changeSelectionCounter($event)"
                  :name="user.username"
                  :email="user.email"
                  :company="user.company.name"
                  :phone="user.phone"
        />
        <!--    No data found message    -->
        <p class="noData" v-if="filteredUser.length === 0">No users where found.</p>
      </div>
    </div>
  </main>
</template>

<script>
import { inject } from "vue";
import UserCard from "./components/UserCard.vue";
export default {
  components: {UserCard},
  data: function () {
    return {
      error: false,
      loading: true,
      userList: [],
      searchQuery: '',
      selected: 0,
      unselected: 0
    }
  },
  computed:{
    filteredUser: function () {
      const query = this.searchQuery.toLowerCase();
      if (this.searchQuery === ""){
        return this.userList;
      }
      return this.userList.filter((user) => {
        return user.username.toLowerCase().includes(query.toLowerCase())
      })
    }
  },
  mounted() {
    this.loadData();
  },
  methods: {
    loadData: function (){
      const axios = inject("$axios");
      axios.get('https://jsonplaceholder.typicode.com/users')
          .then((response) => {
            setTimeout(() => {
              this.userList = response.data;
              this.loading = false;
              this.unselected = response.data.length;
            }, 2000)
          })
          .catch((error) => {
            setTimeout(() => {
              this.error = true;
              this.loading = false;
            }, 2000)
          });
    },
    clearInput: function () {
      this.searchQuery = '';
    },
    changeSelectionCounter: function (event) {
      if (event){
        this.selected ++;
        this.unselected --;
      }else{
        this.selected --;
        this.unselected ++;
      }
    }
  }
};
</script>

<style scoped>
.wrapper div, .noData {
  position: absolute;
  top: 45%;
  left: 45%;
  transform: translate(-50%, -50%);
  text-align: center;
}

.error p {
  margin: auto;
  padding-top: 55px;
  font-size: 32px;
  letter-spacing: 0.5rem;
  color: red;
}
.loader {
  border: 16px solid #f3f3f3;
  border-radius: 50%;
  border-top: 16px solid #3498db;
  width: 150px;
  height: 150px;
  animation: spin 2s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.filter {
  margin: auto;
  text-align: center;
  padding: 25px 10px;
}

.filter input {
  border: 1px solid grey;
  border-radius: 25px;
  padding-left: 10px;
}

.noData{
  left: 50%;
  text-align: center;
  color: red;
  font-size: 18px;
}

</style>
