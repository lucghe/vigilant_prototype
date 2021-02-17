<template>
<div>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">
<div class="container">
  <div class="header">
    <img src="@/assets/Vigilant_Icon_red.png" alt="Vigilant Logo">
    <h1>vigilant</h1>
  </div>
  <div class= "networks-datepicker">
    <p>Select News Network:</p>
    <div class="networks">      
      <p>
        <button class="active" @click="filter = ''">Show all</button>
        <!--<button v-for="article in newsList.articles" :key="article" :class="{ 'active': filter === article.source.name }" @click="filter = article.source.name">{{ article.source.name }}</button>/
        <button v-for="source in newsSources" :key="source" :class="{ 'active': filter === source }" @click="filter = source">{{ source }}</button>/-->
        <button v-for="source in uniqueSources" :key="source" :class="{ 'active': filter === source }" @click="filter = source">{{ source }}</button>
      </p>
    </div>
    <p>Select Date:</p>
    <div class="date">
       <!-- <vueye-datepicker v-model="date" color="#fffff" format="dd/mm/yyyy"/> -->
      <datepicker :inline="true"></datepicker>
      <!-- <datepicker placeholder="Select Date" v-model="vmodelexample"></datepicker>  -->
    </div>
  </div> 
  <div class= "search-box">
      <p>
        <label for="">Search for the following term:</label>
      </p>
      <div class="search-bar">
         <input name=""  type="" placeholder="Search">
          <a class="search-btn" href='#'>
            <i class="fa fa-search"></i>
          </a>
      </div>   
  </div>  
  <div class= "articles">
    <h2>Select Article:</h2>
    <ul class="newsContainer">
      <li v-for="(article, index) in filteredNews" :item="article" :key="index" class="news">
        <a :href="article.url" style="cursor: pointer;">
          <h3>{{ article.title }}</h3>
          <span>
            Source: {{ article.source.name }}<br/>
            Short Description: {{ article.description }}<br/>
            Link: {{ article.url }}
          </span>
        </a>
          <h3>{{ article.title }}</h3>
          <span>
            Source: {{ article.source.name }}<br/>
            Short Description: {{ article.description }}<br/>
            Link: <a :href="article.url" style="cursor: pointer;">{{ article.url }}</a>
          </span>
          <br/><br/>
      </li>
    </ul>
  </div>
</div>
<footer>
<div>© Vigilant 2021 - CDCLab WS20/Practical Software Development & Applied AI WS20</div>
</footer>
</div>
</template>

<script>
// import VueyeDatepicker from 'vueye-datepicker'
import Datepicker from 'vuejs-datepicker';

export default {
  name: 'App',
  data () {
    return {
      date: {
        value:new Date(),
        formattedValue:''
      },       
      newsList: [],
      newsSources: [],   
      filter: ""
    }
  },
  created() {
    var newsAPIURL = "https://newsapi.org/v2/everything?q=trump&from=2021-01-22&to=2021-01-22&sortBy=popularity&apiKey=7f7cf3684558439cbbb596fabb08ae74";    fetch(newsAPIURL)
      .then(res => res.json())
      .then(res => (this.newsList = res))
      .catch(error => console.log(error));
  },
  computed: {
    uniqueSources() {
      this.newsList.articles.forEach(article => {
        if (!this.newsSources.includes(article.source.name)) {
          this.newsSources.push(article.source.name);
        }        
      });
      return this.newsSources;
    },
    filteredNews() {
      if (!this.filter) {
        return this.newsList.articles;
      }
      return this.newsList.articles.filter(p => p.source.name === this.filter);
    }
  },
  methods: {

  },
  watch: {

  },
  components: {
    // VueyeDatepicker
    Datepicker
  }
}
</script>

<style>
*{
  margin: 0;
	padding: 0;
	border: none;
  cursor: default;
}
h1, footer div{
     font-family: Helvetica, sans-serif;
}
@font-face {
  font-family: "Woodford Bourne";
  src: local("Woodford_Bourne"),   url(./assets/Woodford_Bourne/woodfordbourne-regular.otf) format("truetype");
  }
.container{
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-areas: 
        "h h"
        "w a"
        "n a"
        "n a";
  text-align: center;
  /* height: 100vh; */
}
/* #app {
  font-family: Helvetica, sans-serif;
} */
.header{
  grid-area: h;
  background-color: black;
  height: 6rem;
  display: flex;
  align-items: center;
  justify-content: center;
}
img{
  width: auto;
  height: 63px;
  float: left;
  padding: 0 10px 0 0;
}
h1{
  font-size: 36px;
  text-transform: uppercase;
  color: white;
  letter-spacing: 5px;
  text-align: center;
}
div.networks {
  margin:5px; 
  padding:5px; 
  height: 20%;
  overflow: auto;
}
.networks{
  grid-area: n;
  padding: 5%;
  background-color: rgba(195, 204, 204, 0.24);
}
/* .search-box, .networks{
  background-color: #E2E9EA;
} */
.search-box{
  grid-area: w;
  background-color: #cbd9d9ab;
  /* background-color: #E2E9EA; */
  /* padding: 40px; */
  /* height: 80px; */
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.search-bar{
  height: 30px;
  width: auto;
  padding: 0 10px 0 20px;
  background-color: white;
  border: 1px solid #cfd4db;
  border-radius: 2rem;
  font-size: 12px;
  transition: all .2s ease;
  background-size: 1rem;
  display: flex;
  justify-content: center;
  align-items: center;
}
.search-btn{
color: grey;
font-size: 18px;
}
input {
 cursor: text;
 font-size: 15px;
 outline: none;
 transition: all .2s ease;
}
::placeholder {
  color: grey;
  font-size: 1em;
}
ul.newsContainer {
  list-style-type: none;
  margin: 0;
  padding: 1em;
}
div.articles {
  padding:5px; 
  overflow: auto; 
  text-align:justify;
}
.articles{
  grid-area: a;
  background-color: #CBD9D9;
  height: 100vh;
  /* background-color: #9ab2b4; */
  /* padding: 20px; */
  /* border-left: 2px dashed black; */
  /* height: 300px; */
}
.articles p{
  padding: 40px 0 0 0;
}
p{
  font-size: 20px;
  font-family: "Woodford Bourne", Helvetica, sans-serif;
  padding: 10px 0 10px 0;
}
h2{
  font-size: 20px;
  font-family: "Woodford Bourne", Helvetica, sans-serif;
  padding: 10px 0 10px 0;
}
.date{
  /* padding: 80px; */
  display: flex;
  justify-content: center;
  font-family: "Woodford Bourne", Helvetica, sans-serif;
}
button {
  border-radius: 4%;
  cursor: pointer;
  margin: 5px;
  font-size: 17px;
  padding: 3px 10px 3px 10px;
  background-color: #CBD9D9;
  justify-content: center;
}
/* not working */
button :visited {
  background-color: orange;
  }
footer div{
  color: white;
  text-align: center;
  font-size: 15px;
  position: absolute;
  top: 50%;
  left: 50%;
  margin-right: -50%;
  transform: translate(-50%, -50%)  
}
footer{
  position: fixed;
  left: 0;
  bottom: 0;
  width: 100%;
  height: 40px;
  background-color:#2E4950;

}
</style>
