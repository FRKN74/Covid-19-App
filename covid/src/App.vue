<template>

<div :class="theme === 'light' ? 'body-light' : 'body-dark'">
    <nav class="navbar navbar-md " :class="theme === 'light' ? 'nav-light' : 'nav-dark' ">
            <a class=" navbar-brand">Covid-19</a>
        <div class="button-group">
            <button @click="changeTheme"><i class="fas fa-adjust"></i></button>
        </div>
    </nav>
  <div class="container mt-5">

  <div class="data">
    <div class="row">
      <div class="col-md-12">
        <div class="countrySelect">
          <select v-model="selectedCountry"
          class="form-control " :class="theme === 'light' ? 'bg-light' : 'bg-dark text-white'"
          @change="getDataByCountry">
            <option value="">Global</option>
            <option  v-for="country in countries" :value="country.Slug" :key="country.Slug">{{ country.Country }}</option>
          </select>
        </div>
      </div>
    </div>
    <div class="row mt-5">
      <div class="deaths col md-4  ml-3">

        <label class="centerBox">DEATHS <br>
          <small class="text-muted">(Ölen)</small>
        </label>

        <h1 class="dataNubmers">{{ Deaths | numberFilter}}</h1>

      </div>
      <div class="confirmed col md-4 ml-3 ">

        <label class="centerBox">CONFİRMED <br>
          <small class="text-muted">(virüslü)</small>
        </label>

        <h1 class="dataNubmers">{{ Confirmed | numberFilter}}</h1>

      </div>
      <div class="recover col md-4  ml-3">

        <label class="centerBox">RECOVER <br>
          <small class="text-muted">(iyileşen)</small>
        </label>

        <h1 class="dataNubmers">{{ Recovered | numberFilter}}</h1>

      </div>
    </div>

    <div class="row mt-5">
        <table class="table table-striped" :class="theme === 'light' ? 'table-light' : 'table-dark'">
          <thead  :class="theme === 'light' ? 'thead-light' : 'thead-dark'">
              <tr>
                <th scope="col">#</th>
                <th scope="col">Country</th>
                <th scope="col">Deaths</th>
                <th scope="col">Confirmed</th>
                <th scope="col">Recover</th>
              </tr>
          </thead>
          <tbody>
            <tr v-for="(item , index) in data.Countries" :key="item.Slug">
              <th scope="row">{{ index }}</th>
              <td>{{ item.Country | numberFilter}}</td>
              <td>{{ item.TotalDeaths | numberFilter}}</td>
              <td>{{ item.TotalConfirmed | numberFilter}}</td>
              <td>{{ item.NewRecovered | numberFilter}}</td>
              
            </tr>
          </tbody>
        </table>
    </div>
    

  </div>
</div>
</div>

</template>

<script>
import  axios from 'axios';
import navbar from './components/navbar.vue';
export default {
  data() {
    return {
      selectedCountry : '',
      countries : {},
      data : {},
      Confirmed : 0 ,
      Deaths : 0,
      Recovered : 0,
      theme : "light",
    }
  },
  components:{
    appNavbar : navbar
  },
  filters : {
    numberFilter(number){
      return number.toLocaleString();
    }
  },
  methods: {
    changeTheme(){
      console.log("asdasd");
      this.theme = this.theme === "dark" ? "light" : "dark";
      this.body = this.body === "dark" ? "light" : "dark";
    },
    getCounties(){
      
      axios.get('https://api.covid19api.com/countries').then( Response => {
        this.countries = Response.data;
      });
    },
    getSummery(){
      axios.get('https://api.covid19api.com/summary').then( Response => {

        const data = Response.data;
          this.data = data;

          this.Confirmed = data.Global.TotalConfirmed;
          this.Deaths = data.Global.TotalDeaths;
          this.Recovered =data.Global.TotalRecovered;
          
      });
    },
    getDataByCountry(){
        let data = this.data.Countries.filter(country => {
          return country.Slug == this.selectedCountry;
        });
        console.log(data);
        data = data[0];
          this.Confirmed = data.TotalConfirmed;
          this.Deaths = data.TotalDeaths;
          this.Recovered =data.TotalRecovered;
          
        
        
    },
  },
  mounted() {
    this.getCounties();
    this.getSummery();
    this.getDataByCountry();
  },
}
</script>

<style>
:root{
    --navlight : wheat;
    --navdark : darkgrey;


    --lightThema : white;
    --lightThemaText : black;
    
    --darkThema : rgb(75, 75, 75);
    --darkThemaText: white;

}
body{
  transition: all 1s;
}
.body-light{
  background-color: var(--lightThema); 
  color:var(--lightThemaText);
  transition: 1s;
}
.body-dark{
  background-color: var(--darkThema); 
  color:var(--darkThemaText);
  transition: 1s;
}

.nav-light{
    background-color: var(--navlight);
    color:var(--lightThemaText) ;
    transition: 1s;
}
.nav-dark{
    background-color: var(--navdark);
    color:var(--darkThemaText) ;
    transition: 1s;
}
table{
    transition: all 1s;
}
.deaths{
  height: 200px;
  background-color: brown;
  border-radius: 10px;
  transition: all 1s;
  
}
.deaths:hover{
    background-color: darkred;
  transform: scaleY(1.1);
  transition: all 1s;
  
}
.confirmed{

  height: 200px;
  background-color: rgb(179, 164, 37);
  border-radius: 10px;
  transition: all 1s;
}
.confirmed:hover{
  background-color: darkkhaki;
  transform: scaleY(1.1);
  transition: all 1s;
}
.recover{
  height: 200px;
  background-color: rgb(42, 165, 42);
  border-radius: 10px;
  transition: all 1s;
}
.recover:hover{
  background-color: darkgreen;
  transform: scaleY(1.1);
  transition: all 1s;
}
.centerBox{
color: white;
margin-top: 15%;
margin-left: 40%;

}
.dataNubmers{
  text-align: center;
}


</style>
