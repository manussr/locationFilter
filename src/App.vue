<template>
  <div :class="{extmargin:mode==='minimised'}">
    <div id="app"  :class="{rotated:mode==='minimised'}">
  <div >
    <!--Antes de la barra-->
      <div class="container">
      <div class="row red">
          <div class="col-sm-10" :class="{redwidth:mode==='minimised'}">
            <h4>Locations</h4>
          </div>
          <!--Cambiar estados 1-->
          <div v-if="mode==='default'">
          <div class="col-sm-1 right-padd">
              <i class="fas fa-window-minimize blue-icon" @click="mode='collapsed'"></i>
          </div>
          <div class="col-sm-1 left-padd">
              <i class="fas fa-arrow-to-left  blue-icon" @click="mode='minimised'" data-toggle="collapse" data-target=".content"></i>
          </div>
          </div>
          <!--2-->
          <div v-else-if="mode==='minimised'">
          <div class="col-sm-1 left-padd">
              <i class="fas fa-arrow-to-bottom blue-icon" @click="mode='default'"></i>
          </div>
          </div>
          <!--3-->
          <div v-else>
          <div class="col-sm-1 left-padd">
              <i class="fas fa-window-maximize blue-icon" @click="mode='default'"></i>
          </div>
          <div class="col-sm-1 left-padd">
              <i class="fas fa-arrow-to-left blue-icon" @click="mode='minimised'"></i>
          </div>
          </div>      
      </div>
    </div>
    <div :class="{display:mode==='minimised'||mode==='collapsed'}">
      <!--Barra de búsqueda-->
    <div class="input-icons">
    <span class="fa-stack">
              <i class="fas fa-square fa-stack-2x"></i>
              <i class="fas fa-search fa-stack-1x search-blue "></i>
            </span>
      <input class="input-field" type="text" v-model="search" placeholder="Filter Locations">
    </div>
      <!--Antes del botón-->
      <button @click="unselectAll" class="btn btn-link"> <i class="fa fa-times" aria-hidden="true"></i> Clear All</button>
      <ol class="list-wrapper">
        <li v-for="location in filteredList">
          <label>
            <input type="checkbox" v-model="locationIds" :value="location.id">
            <img :src="`https://www.countryflags.io/${location.code}/flat/16.png`">
            <span>
              {{ location.city }}-{{location.country}}
            </span>
          </label>
        </li>
      </ol>  
    </div>
    
    </div>
  </div> 
  </div>  
</template>

<script>
export default {
 data: function() {
    return{
    mode: 'default',
    search: '',
    locations:[
    	{id:0, city:"Alberta", country:"Canada"},
    	{id:1, city:"Bangalore", country:"India"},
      {id:2, city:"Kiev", country:"Ukraine"},
      {id:3, city:"Pune", country:"India"},
      {id:4, city:"Krakow", country:"Poland"},
      {id:5, city:"Beijing", country:"China"},
      {id:6, city:"Kiev", country:"United Kingdom"},
      {id:7, city:"Moscow", country:"Russia"},
      {id:8, city:"Warsaw", country:"Poland"},
      {id:9, city:"Lion", country:"France"}
    ],
    locationIds:[]
    }
  },
  methods: {
  		sortList(){
      console.log(this.locations);
    	this.locations.sort((a,b)=> a.city.concat("-").concat(a.country) < b.city.concat("-").concat(b.country) ? -1 : a.city.concat("-").concat(a.country) > b.city.concat("-").concat(b.country) ? 1 : 0);
    },
    unselectAll(){
    	this.locationIds = [];
      for (let user in this.users) {
      	console.log(user);
        console.log(this.users[user].id.toString())
        this.userIds.pop(this.users[user].id.toString());
       	}
     },
      fetchData(){
       let self=this;
       fetch("./public/country_data.json")
        .then(response => {
            return response.json()
        })
        .then(data => {
            // Work with JSON data here
            for(let i of self.locations){
                for(let j of data){
                  if(i.country===j["Name"]){
                    i.code=j["Code"].toLowerCase();
                  }
              }
            }
            console.log("Done");
        })
        .catch(err => {
            // Do something for an error here
            console.log("Error fetching data")
        })
     }
  },
  computed: {
    filteredList() {
    	this.sortList();
      return this.locations.filter(location => {
        return location.city.toLowerCase().includes(this.search.toLowerCase())
      })
    }
  },
   created(){
     this.fetchData();
  }
}
</script>

<style>
    body {
    background: #20262E;
    padding: 20px;
    font-family: Helvetica;
    width: 70%;
  }

  #app {
    background: #F0F0F0;
    border-radius: 4px;
    padding: 20px;
    transition: all 0.2s;
    width:45%;
  }

  li {
    margin: 8px 0;
    list-style-type: none;
  }

  h2 {
    font-weight: bold;
    margin-bottom: 15px;
  }

  del {
    color: rgba(0, 0, 0, 0.3);
  }

  ol{
    width: 60%;
    margin: auto;
  }

  .blue-color{
    color: blue;
    background-color: white;
  }

  .no-border {
      border: 0;
      box-shadow: none; 
  }

  /*
  Estilos para la entrada
  */
  .input-icons span{
    position: absolute; 
  }
  
  .search-blue{
    color:lightBlue;
  }
        
  .input-icons { 
    width: 100%; 
    margin-bottom: 10px; 
  } 
            
  .icon { 
    padding: 10px; 
    min-width: 40px; 
  } 
            
  .input-field { 
    width: 100%; 
    padding: 4px 40px; 
    text-align: left; 
  }
          
  .fa-square {
    color: white;
  }

  .red{
    width: 350px;
  }

  .left-padd{
  padding-right: 5px;
  padding-top: 5px;
  }

  .right-padd{
    padding-right:5px;
    padding-top:5px;
  }

  .blue-icon{
    color:blue;
  }

  div + h3{
    color: red;
  }
  .rotated {
    -webkit-transform: rotate(-90deg);
    -moz-transform: rotate(-90deg);
    -o-transform: rotate(-90deg);
    -ms-transform: rotate(-90deg);
    transform: rotate(-90deg);
    width: 100%;
    
  }

  .display{
    display: none;
  }

  .redwidth{
    width:100px;
  }

  .extmargin{
    margin: 100px 200px;
  }
</style>
