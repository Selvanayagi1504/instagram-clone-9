<template>
  <div class="app">
    <div class="nav-edit">
      <div class="row" style="margin-top: 7px;margin-bottom: 5px;">
        <div class="col-sm-4">
          <img :src="require('./images/instagram-name.png')" class="img-insta" />
        </div>
        <div class="col-sm-4">
          <!-- <input type="text" placeholder="&#xF002;Search" class="search-bar" /> -->
          <center>
            <div class="form-group has-search">
              <span class="fa fa-search form-control-feedback"></span>
              <input type="text" class="form-control search-bar" placeholder="Search">
            </div>
          </center>
           
        </div>
        <div class="col-sm-4" style="margin-left: -3%;margin-top: 0.5%;" v-bind:key="pro">
          <router-link to="/upload">
            <img :src="require('./images/home-icon.png')" class="icon-side" />
          </router-link>
          <img :src="require('./images/save.png')" class="icon-side" />
          <img :src="require('./images/discover.png')" class="icon-side" />
          <img :src="require('./images/activity.png')" class="icon-side" />
          <router-link to="/profile">
            <img :src="`${pro}`" style="width: 7%;border-radius: 16px;" class="icon-side" />
          </router-link>
          <router-link to="/movieapp">
            <img :src="require('./images/movie.png')" style="width: 7%;border-radius: 16px;" class="icon-side" />
          </router-link>
        </div>
      </div>
    </div>
    <Search :search="state.search" @search="handleSearch" />
    <div class="movies">
      <Movie v-for="movie in state.movies" :movie="movie" :key="movie.imdbID" />
    </div>
    <div class="favourites">
      <img :src="require('./images/fav.jpeg')" style="width: 4%;" />Favourites
      <br>
      <center>
        <table id="post" cellspacing="5">
          <tr v-bind:key="row" v-for="row in rows">
            <!-- <div v-bind:key="titles" v-for="titles in title"> -->
            <td class="size">
              <img :src="`${row.id}`" class="size" />
              <!-- <p class="mov-tit">
                      {{titles.id}}
                    </p> -->
            </td>
            <td class="size">
              <img :src="`${row.name}`" class="size" />
              <!-- <p class="mov-tit">
                      {{titles.name}}
                    </p>                     -->

            </td>
            <td class="size">
              <img :src="`${row.phone}`" class="size" />
              <!-- <p class="mov-tit">
                      {{titles.phone}}
                    </p>                     -->
            </td>
            <!-- </div> -->
          </tr>
        </table>
      </center>
    </div>
    <div class="footer">
      <p>
        <a href="">ABOUT</a>
        <a href="">HELP</a>
        <a href="">PRESS</a>
        <a href="">API</a>
        <a href="">JOBS</a>
        <a href="">PRIVACY</a>
        <a href="">TERMS</a>
        <a href="">LOCATIONS</a>
        <a href="">HASHTAGS</a>
        <a href="">TOP ACCOUNTS</a>
        <a href="">LANGUAGE</a>
        <a href="" class="disabled">&#169;2020 INSTAGRAM FROM FACEBOOK</a>
      </p>
    </div>
  </div>
</template>

<script>
  //   import Header from './Header.vue';
  import Search from './Search.vue';
  import Movie from './Movie.vue';
  import {
    useMovieApi
  } from '../Hooks/movie-api.js';

  export default {
    name: 'app',
    components: {
      Search,
      Movie
    },
    setup() {
      const state = useMovieApi();

      return {
        profile_path: [],
        pro: "",
        state,
        rows: [],
        title: [],
        handleSearch(searchTerm) {
          state.loading = true;
          state.search = searchTerm;
        }
      };
    },
    mounted() {
      let email = sessionStorage.getItem('email');
      let users = JSON.parse(localStorage.getItem("instausers"));
      console.log(email)
      users.forEach(user => {
        if ((user.moboremail == email) || (user.uname == email) || (user.email == email)) {
          this.profile_path = user.profile
          this.pro = user.profile
          let i = 1;
          let k = [];
          let kt = [];
          var s;
          var m;
          var st;
          var mt;
          user.fav.forEach(f => {
            if (i == 1) {
              s = f.poster
              st = f.title
              k = []
              kt = []
              k = {
                id: s
              }
              kt = {
                id: st
              }
            } else if (i == 2) {
              m = f.poster
              mt = f.title
              k = []
              kt = []
              k = {
                id: s,
                name: m
              }
              kt = {
                id: st,
                name: mt
              }

            } else if (i == 3) {
              k = []
              kt = []
              k = {
                id: s,
                name: m,
                phone: f.poster
              }
              kt = {
                id: st,
                name: mt,
                phone: f.title
              }

            }
            i++;
            // console.log(i)
            // console.log(s)
            if (i == 4) {
              s = ""
              m = ""
              st = ""
              mt = ""
              i = 1;
              this.rows.push(k)
              this.title.push(kt)
              console.log(this.rows)
              k = []
              kt = []
            }
          })
          if (i > 1) {
            // i--;
            if (i == 2) {
              // m="https://i.pinimg.com/originals/aa/bf/c8/aabfc8cd95f0350be64a0f300ecb111e.jpg"
              k = []
              kt = []
              k = {
                id: s,
                name: "https://convertingcolors.com/plain-FAFAFA.svg",
                phone: "https://convertingcolors.com/plain-FAFAFA.svg"
              }
              kt = {
                id: st,
                name: "",
                phone: ""
              }

            } else if (i == 3) {
              k = []
              kt = []
              k = {
                id: s,
                name: m,
                phone: "https://convertingcolors.com/plain-FAFAFA.svg"
              }
              kt = {
                id: st,
                name: mt,
                phone: ""
              }

            }
            this.rows.push(k)
            this.title.push(kt)
          }
          // let i = 1;
          // let k = [];
          // var s;
          // var m;
          // let kt = [];
          // var st;
          // var mt;
          // user.fav.forEach(f => {
          //     if (i == 1) {
          //         s = f.poster
          //         k = []
          //         k = {
          //             id: s
          //         }
          //         st = f.title
          //         kt = []
          //         kt = {
          //             id: st
          //         }
          //     } else if (i == 2) {
          //         m = f.poster
          //         k = []
          //         k = {
          //             id: s,
          //             name: m
          //         }
          //         mt = f.title
          //         kt = []
          //         kt = {
          //             id: st,
          //             name: mt
          //         }

          //     } else if (i == 3) {
          //         k = []
          //         k = {
          //             id: s,
          //             name: m,
          //             phone: f.poster
          //         }
          //         kt = []
          //         kt = {
          //             id: st,
          //             name: mt,
          //             phone: f.title
          //         }
          //     }
          //     i++;
          //     console.log(i)
          //     console.log(s)
          //     if (i == 4) {
          //         s = ""
          //         m = ""
          //         i = 1;
          //         this.rows.push(k)
          //         console.log(this.rows)
          //         k = []
          //         st = ""
          //         mt = ""
          //         this.titles.push(kt)
          //         console.log(this.title)
          //         kt = []
          //     }
          // })
          // if (i > 1) {
          //     // i--;
          //     if (i == 2) {
          //         // m="https://i.pinimg.com/originals/aa/bf/c8/aabfc8cd95f0350be64a0f300ecb111e.jpg"
          //         k = []
          //         k = {
          //             id: s,
          //             name: "https://convertingcolors.com/plain-FAFAFA.svg",
          //             phone: "https://convertingcolors.com/plain-FAFAFA.svg"
          //         }
          //         kt = []
          //         kt = {
          //             id: st,
          //             name: "",
          //             phone: ""
          //         }

          //     } else if (i == 3) {
          //         k = []
          //         k = {
          //             id: s,
          //             name: m,
          //             phone: "https://convertingcolors.com/plain-FAFAFA.svg"
          //         }
          //         kt = []
          //         kt = {
          //             id: st,
          //             name: mt,
          //             phone: ""
          //         }

          //     }
          //     this.rows.push(k)
          //     this.titles.push(kt)
          // }
          // console.log(this.rows)
          // this.profile_path = user.profile

        }
      })
    },
    methods: {

    }
  }
</script>

<style>
  .app {
    text-align: center;
    overflow-x: hidden;
  }

  .header {
    background-color: #282c34;
    height: 70px;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    font-size: calc(10px + 2vmin);
    color: white;
    padding: 20px;
    cursor: pointer;
  }

  .spinner {
    height: 80px;
    margin: auto;
  }

  .intro {
    font-size: large;
  }

  /* new css for movie component */

  * {
    box-sizing: border-box;
  }

  .movies {
    display: flex;
    flex-wrap: wrap;
    flex-direction: row;
  }

  .header h2 {
    margin: 0;
  }

  .add-movies {
    text-align: center;
  }

  .add-movies button {
    font-size: 16px;
    padding: 8px;
    margin: 0 10px 30px 10px;
  }

  .movie {
    padding: 5px 25px 10px 25px;
    max-width: 25%;
  }

  .errorMessage {
    margin: auto;
    font-weight: bold;
    color: rgb(161, 15, 15);
  }


  .search {
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: center;
    margin-top: 10px;
  }


  input[type="submit"] {
    padding: 5px;
    background-color: transparent;
    color: black;
    border: 1px solid black;
    width: 80px;
    margin-left: 5px;
    cursor: pointer;
  }


  /* input[type="submit"]:hover {
    background-color: #282c34;
    color: antiquewhite;
  } */


  .search>input[type="text"] {
    width: 40%;
    min-width: 170px;
    border-radius: 4px;
    border: 1px solid;
  }

  @media screen and (min-width: 694px) and (max-width: 915px) {
    .movie {
      max-width: 33%;
    }
  }

  @media screen and (min-width: 652px) and (max-width: 693px) {
    .movie {
      max-width: 50%;
    }
  }


  @media screen and (max-width: 651px) {
    .movie {
      max-width: 100%;
      margin: auto;
    }
  }

  .footer {
    margin-top: 6em;
    margin-left: 8em;
    font-size: 12px;
  }

  .footer a {
    color: #385185;
    margin-right: 1em;
    font-weight: 600;
  }

  a.disabled {
    pointer-events: none;
    cursor: default;
    margin-left: 20em;
  }

  .changepoto {
    overflow-x: hidden;
  }

  .nav-edit {
    background-color: white;
    border: 1px solid rgba(var(--ca6, 219, 219, 219), 1);
  }

  .search-bar {
    text-align: center;
    background-color: rgba(var(--b3f, 250, 250, 250), 1);
    border: 1px solid rgba(var(--ca6, 219, 219, 219), 1);
    border-radius: 2px;
    font-family: 'Helvetica', FontAwesome, sans-serif;
    width: 53%;
    font-size: 12px;
    height: 74%;
    margin-top: 1%;
  }

  .icon-side {
    width: 6%;
    margin-right: 3%;
  }

  .img-insta {
    width: 27%;
  }

  input:focus,
  textarea:focus,
  select:focus {
    outline: none;
  }

  .size {
    height: 300px;
    width: 300px;
  }

  .mov-tit {
    text-align: center;
    font-size: 16px;
    margin-top: 6%;
    color: black;
  }

  .main {
      width: 50%;
      margin: 50px auto;
  }

  .has-search .form-control {
      padding-left: 2.375rem;
  }

  .has-search .form-control-feedback {
      position: absolute;
      z-index: 2;
      display: block;
      width: 2.375rem;
      height: 2.375rem;
      line-height: 1.7em;
      text-align: center;
      pointer-events: none;
      color: #aaa;
      margin-left: 38%;
  }
  .form-control:focus {
      background-color: rgba(var(--b3f, 250, 250, 250), 1);
      border: 1px solid rgba(var(--ca6, 219, 219, 219), 1);
      outline: none;
      box-shadow: none;
  }
</style>