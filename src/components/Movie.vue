<template>
  <div class="movie">
    <h2>{{ movie.Title }}</h2>
    <div>
      <img width="200" :alt="altText" :src="movie.Poster" />
    </div>
    <p>{{ movie.Year }}</p>
    <!-- <button @click="add(`${movie.Title}`,`${movie.Poster}`,`${movie.Year}`)">Add to favourites</button> -->
    <button @click="add(`${movie.Title}`,`${movie.Poster}`,`${movie.Year}`)" v-show="!isFavorite">Add to favorites</button>
    <button @click="remove(`${movie.Title}`)" v-show="isFavorite">Delete from favorites</button>
  </div>
</template>

<script>
  import { computed } from '@vue/composition-api';

  export default {
    name: "Movie",
    props: ['movie'],
    
    setup({ movie }) {
      
      const altText = computed(() => `The movie titled: ${movie.Title}`);
      console.log(movie);
      var t=false
      let email = sessionStorage.getItem('email');
      let users = JSON.parse(localStorage.getItem("instausers"));
       users.forEach(user => {
          if ((user.moboremail == email) || (user.uname == email) || (user.email == email)) {
            user.fav.forEach(f => {
              if(movie.Title==f.title){
                t=true
              }
            })
          }
      })
      return { altText,movies:[],isFavorite:t };
    },
    methods:{
      remove(title){
        this.isFavorite=false
        let email = sessionStorage.getItem('email');
        let users = JSON.parse(localStorage.getItem("instausers"));
        this.movies=[]
        users.forEach(user => {
            if ((user.moboremail == email) || (user.uname == email) || (user.email == email)) {
              user.fav.forEach(f => {
                if(title!=f.title){
                  this.movies.push(f)
                }
              })
              alert("movie deleted from favourites")
              this.saveimage()
            }
        })
      },
    add(title,poster,year) {
      this.isFavorite=true
      this.movies=[]
      let email = sessionStorage.getItem('email');
      let users = JSON.parse(localStorage.getItem("instausers"));
      users.forEach(user => {
          if ((user.moboremail == email) || (user.uname == email) || (user.email == email)) {
            this.movies=user.fav
          }
      })
      var movie = {
          title: title,
          poster: poster,
          year: year
      }
      console.log(movie)
      this.movies.push(movie)
      alert("movie added to favourites")
      this.saveimage();
    },
    saveimage() {
      let email = sessionStorage.getItem('email');
      let users = JSON.parse(localStorage.getItem("instausers"));
      let newusers = [];
      users.forEach(user => {
          if ((user.moboremail == email) || (user.uname == email) || (user.email == email)) {
              let cuser = {
                  moboremail: user.moboremail,
                  fname: user.fname,
                  uname: user.uname,
                  pass: user.pass,
                  phone: user.phone,
                  email: user.email,
                  website: user.website,
                  bio: user.bio,
                  gender: user.gender,
                  profile: user.profile,
                  posts: user.posts,
                  fav:this.movies
              }
              newusers.push(cuser);
          } else {
              newusers.push(user);
          }
      })
      console.log(newusers)
      localStorage.setItem("instausers", JSON.stringify(newusers));
    }
    }
  };
</script>
