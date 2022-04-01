<template>
<form  @submit.prevent="searchNews" class="d-flex flex-column justify-content-center">
    <div class="input-group mx-sm-3 mb-2">
        <label class="visually-hidden" for="search">Search</label>
        <input type="search" name="search" v-model="searchTerm"
        id="search" class="form-control mb-2 mr-sm-2" placeholder="Enter
        search term here" />
        <button class="btn btn-primary mb-2">Search</button>
    </div>
    <p>You are searching for {{ searchTerm }}</p>
 </form>
    <div class = "card-group">
    <div v-for="article in articles" class="container_view">
    <div class="card">
        <div class = "grid_view">
        <div class="card text-left" style="width: 18rem;">
            
            <img class="card-img-top" :src=" article.urlToImage " alt="Card image" width="250" height="200">
            
            <div class="card-body">
            <h5 class="card-title"> {{ article.title }} </h5>
            <p class="card-text"></p>
            </div>
            <div class="card-body">
                <p> {{ article.description }} </p>
            </div>   
            <br>
            <div class="card-body">
                <div class="d-grid gap-2">
                    <a :href=" article.url " id ="btn" class="btn btn-lg btn-primary">View Link To Article</a>
                </div>
            </div>
        </div>
    </div> 
  </div>  
</div>
</div>


</template>

<script>
    export default {
    data() {
        return {
            articles: [],
            searchTerm: ''
            
        }
        },
        created() {
            let self = this;

            fetch('https://newsapi.org/v2/top-headlines?country=us',
{
 headers: {
 Authorization: `Bearer ${import.meta.env.VITE_NEWSAPI_TOKEN}`

 }
})
 .then(function(response) {
 return response.json();
 })
 .then(function(data) {
 console.log(data);
 self.articles = data.articles;

 });
 },
 methods: {
    searchNews() {
                let self = this;
                fetch('https://newsapi.org/v2/everything?q='+self.searchTerm + '&language=en', {
                headers: {'Authorization': `Bearer ${import.meta.env.VITE_NEWSAPI_TOKEN}`,
                }
                })
        .then(function(response) {
            return response.json();
        })
        .then(function(data) {
            console.log(data);
            self.articles = data.articles;
        });
    }
 }
};
</script>

<style>

  .grid_view{
      margin: 5px;
  }
  h5{
     font-weight: bold; 
  }
</style>