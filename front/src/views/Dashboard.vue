<template>
  <div id="dashboard">
    <Navbar />
    <TabBar />
    <div class="card m-3" style="background-color: #f4f4f4e3">
      <div class="card-header root">
        <h1 class="text text-center">My Favorites Sports</h1>
      </div>
      <div class="card-body favorite">
        <div v-if="mySports.length == 0" class="row mt-2 justify-content-center">
          <label class="card-text text-center">No favorite sports added</label>
        </div>
        <div v-else>
          <div v-for="sport in mySports" v-bind:key="sport.name" class="row mt-2 p-3 justify-content-center">
            <a class="btn btn-default" @click="goTo(sport.name)">
              <h4>{{ sport.name }}</h4>
            </a>
            <button class="btn btn-danger btn-sm rounded-circle ml-5 mb-2" @click="deleteSport(sport._id)">
              Del
            </button>
          </div>
        </div>
      </div>
    </div>
    <div class="card m-3" style="background-color: #f4f4f4e3">
      <div class="card-header root">
        <h1 class="text text-center">My Widgets</h1>
      </div>
      <div class="card-body dash m-0">
        <div v-if="myFavorites.length == 0">You don't have favorite</div>
        <div v-else>
          <div class="card-deck m-0">
            <div v-for="favorite in myFavorites" :key="favorite._id">
              <DisplayLive v-if="
                favorite.data[0].type == 'component' &&
                favorite.data[0].name == 'live' &&
                (favorite.data[0].sport == 'CS-GO' ||
                  favorite.data[0].sport == 'LOL'||
                  favorite.data[0].sport == 'DOTA2')
              " :id="favorite._id" :sport="favorite.data[0].sport" :apiName="favorite.data[0].apiName"
                :delButton="true" v-on:delfavorite="delToMyFavorites" />
              <FootballLiveFavori v-if="
                favorite.data[0].type == 'live' &&
                favorite.data[0].sport == 'football'
              " :id="favorite._id" :league_id="favorite.data[0].league_id" v-on:delfavorite="delToMyFavorites" />
              <DisplayLastResults v-if="
                favorite.data[0].type == 'component' &&
                favorite.data[0].name == 'lastResult' &&
                (favorite.data[0].sport == 'CS-GO' ||
                  favorite.data[0].sport == 'LOL'||
                  favorite.data[0].sport == 'DOTA2')
              " :id="favorite._id" :sport="favorite.data[0].sport" :apiName="favorite.data[0].apiName"
                :delButton="true" v-on:delfavorite="delToMyFavorites" />
              <DisplayCalendar v-if="
                favorite.data[0].type == 'component' &&
                favorite.data[0].name == 'calendar' &&
                (favorite.data[0].sport == 'CS-GO' ||
                  favorite.data[0].sport == 'LOL'||
                  favorite.data[0].sport == 'DOTA2')
              " :id="favorite._id" :sport="favorite.data[0].sport" :apiName="favorite.data[0].apiName"
                :delButton="true" v-on:delfavorite="delToMyFavorites" />
              <DisplayListTeam v-if="
                favorite.data[0].type == 'component' &&
                favorite.data[0].name == 'list' &&
                (favorite.data[0].sport == 'CS-GO' ||
                  favorite.data[0].sport == 'LOL'||
                  favorite.data[0].sport == 'DOTA2')
              " :id="favorite._id" :sport="favorite.data[0].sport" :apiName="favorite.data[0].apiName"
                :delButton="true" v-on:delfavorite="delToMyFavorites" />
              <DisplayRanking v-if="
                favorite.data[0].type == 'component' &&
                favorite.data[0].name == 'ranking' &&
                (favorite.data[0].sport == 'CS-GO' ||
                  favorite.data[0].sport == 'LOL'||
                  favorite.data[0].sport == 'DOTA2')
              " :id="favorite._id" :sport="favorite.data[0].sport" :apiName="favorite.data[0].apiName"
                :delButton="true" v-on:delfavorite="delToMyFavorites" />
              <FavoriteRanking v-if="
              favorite.data[0].type == 'component' &&
              favorite.data[0].name == 'favoriteRanking' &&
                (favorite.data[0].sport == 'CS-GO' ||
                  favorite.data[0].sport == 'LOL'||
                  favorite.data[0].sport == 'DOTA2')
              " :id="favorite._id" :id_tournament="favorite.data[0].id_tournament"
                :leagueName="favorite.data[0].leagueName" v-on:delfavorite="delToMyFavorites" />
            </div>
          </div>
          <div class="card-deck m-0">
            <div v-for="favorite in myFavorites" :key="favorite._id">
              <EsportTeam v-if="
                favorite.data[0].type == 'team' &&
                (favorite.data[0].sport == 'CS-GO' ||
                  favorite.data[0].sport == 'LOL'||
                  favorite.data[0].sport == 'DOTA2')
              " :id="favorite._id" 
              :name="favorite.data[0].name" 
              :apiName="favorite.data[0].sport"
              v-on:delfavorite="delToMyFavorites" />
            </div>
          </div>
          <div class="card-deck m-0">
            <div v-for="favorite in myFavorites" :key="favorite._id">
              <FootballTeamFavorite v-if="
              favorite.data[0].type == 'team' &&
              favorite.data[0].sport == 'football'
              " :id="favorite._id" :id_tournament="favorite.data[0].id_tournament" :team_id="favorite.data[0].team_id"
                v-on:delfavorite="delToMyFavorites" />
            </div>
          </div>
          <div class="card-deck m-0">
            <div v-for="favorite in myFavorites" :key="favorite._id">
              <DisplayFavoriteMatch v-if="
                favorite.data[0].type == 'matches'&&
                (favorite.data[0].sport == 'CS-GO' ||
                  favorite.data[0].sport == 'LOL'||
                  favorite.data[0].sport == 'DOTA2')
              " :id="favorite._id" :matchId="favorite.data[0].match_id" :sport="favorite.data[0].sport"
                :apiName="favorite.data[0].apiName" v-on:delfavorite="delToMyFavorites" />
            </div>
          </div>
          <div class="card-deck m-0">
            <div v-for="favorite in myFavorites" :key="favorite._id">
              <FootballMatchFavori v-if="
                favorite.data[0].type == 'matchFavori' &&
                favorite.data[0].sport == 'football'
              " :id="favorite._id" :match_id="favorite.data[0].match_id" v-on:delfavorite="delToMyFavorites" />
            </div>
          </div>
          <div class="card-deck m-0">
            <div v-for="favorite in myFavorites" :key="favorite._id">
              <DisplayFavoriteNews v-if="
                favorite.data[0].type == 'news'" :id="favorite._id" :newsId="favorite.data[0].name"
                v-on:delfavorite="delToMyFavorites" />
            </div>
          </div>
          <div class="card-deck m-0">
            <div v-for="favorite in myFavorites" :key="favorite._id">
              <DisplayFavoriteNews v-if="
                favorite.data[0].type == 'news'" :id="favorite._id" :newsId="favorite.data[0].name"
                v-on:delfavorite="delToMyFavorites" />
            </div>
          </div>
          <div class="card-deck m-0">
            <div v-for="favorite in myFavorites" :key="favorite._id">
              <FootballTournament v-if="
                favorite.data[0].type == 'tournament' &&
                favorite.data[0].sport == 'football'
              " :id="favorite._id" :id_tournament="favorite.data[0].id_tournament"
                v-on:delfavorite="delToMyFavorites" />
            </div>
          </div>
          <div class="card-deck m-0">
            <div v-for="favorite in myFavorites" :key="favorite._id">
              <FootballCalendarFav v-if="
                favorite.data[0].type == 'calendar' &&
                favorite.data[0].sport == 'football'
              " :id="favorite._id" :id_tournament="favorite.data[0].id_tournament"
                v-on:delfavorite="delToMyFavorites" />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import ENV, {
    API_BACKEND
  } from "../../env.config";
  import Navbar from "@/components/NavBar.vue";
  import TabBar from "@/components/TabBar.vue";

  import DisplayLive from "@/components/DisplayLive.vue";
  import DisplayLastResults from "@/components/DisplayLastResults.vue";
  import DisplayCalendar from "@/components/DisplayCalendar.vue";
  import DisplayListTeam from "@/components/DisplayListTeam.vue";
  import DisplayRanking from "@/components/DisplayRanking.vue";
  import FootballTournament from "@/components/FootballTournament.vue";
  import FootballCalendarFav from "@/components/FootballCalendarFav.vue";
  import FootballTeamFavorite from "@/components/FootballTeamFavorite.vue";
  import FavoriteRanking from "@/components/FavoriteRanking.vue";
  import FootballLiveFavori from "@/components/FootballLiveFavori.vue";
  import FootballMatchFavori from "@/components/FootballMatchFavori.vue";
  import EsportTeam from "@/components/EsportTeam.vue";
  import DisplayFavoriteMatch from "@/components/DisplayFavoriteMatch.vue";
  import DisplayFavoriteNews from "@/components/DisplayFavoriteNews.vue";

  /**
   * View where you can find all tyour favorite's components and your favorite's sports
   * @displayName Dashboard
   */
  export default {
    name: "Dashboard",
    components: {
      Navbar,
      TabBar,
      DisplayLive,
      DisplayLastResults,
      DisplayCalendar,
      DisplayListTeam,
      DisplayRanking,
      EsportTeam,
      FootballTournament,
      FootballCalendarFav,
      FootballTeamFavorite,
      FavoriteRanking,
      DisplayFavoriteMatch,
      DisplayFavoriteNews,
      FootballLiveFavori,
      FootballMatchFavori,

    },
    data() {
      return {
        isLoading: true
      };
    },
    /**
     * This hook is used to redirect user not connected and dispatch 'getMySports' and 'getMyFavorites'
     * A user is considere as not connected if $store.state.UserData.id !=''
     * @public
     */
    async mounted() {
      if (this.$store.state.UserData.id == "") {
        this.$router.push({
          path: "/login",
        });
      }
      this.$store.dispatch("getMySports");
      this.$store.dispatch("getMyFavorites");
    },
    computed: {
      mySports: function () {
        return this.$store.state.MySports;
      },
      myFavorites: function () {
        return this.$store.state.MyFavorites;
      },
    },
    methods: {
      /**
       * This method is used to redirect the user in the view desired. It's save in $store.state.tabSelected the id and name of the next view
       * @param {string} nameTab This param is a the name present in the collection <sport>
       * @public
       */
      goTo(nameTab) {
        let idTab = "";
        this.$store.state.sports.forEach((sport) => {
          if (sport.name == nameTab) {
            idTab = sport._id;
          }
        });
        this.$store.commit("setTabSelected", {
          id: idTab,
          name: nameTab
        });
        this.$router.push({
          name: nameTab
        });
      },

      /**
       * This method is used to delete a sport in your favorite's sports
       * @param {number} id This param means _id present in the collection <MySport>
       * @public
       */
      async deleteSport(id) {
        const body = {
          id: id,
        };
        let mySports = [];
        await fetch(`http://${ENV.API_BACKEND}/mysports`, {
            method: "DELETE",
            headers: {
              "content-type": "application/json",
              authorization: "Bearer " + this.$store.state.access_token,
            },
            body: JSON.stringify(body),
          })
          .then((res) => res.clone().json())
          .then((json) => (mySports = json));
        this.$store.dispatch("getMySports");
      },
      /**
       * This method is used to delete a favorite's component in the favorite's user
       * @param {number} id  This param means _id present in the collection <MyFavorite>
       * @public
       */
      async delToMyFavorites(id) {
        this.$store.commit("setAccessToken");
        if (this.$store.state.access_token != "") {
          await fetch(`http://${ENV.API_BACKEND}/myfavorites`, {
            method: "DELETE",
            headers: {
              "content-type": "application/json",
              authorization: "Bearer " + this.$store.state.access_token,
            },
            mode: "cors",
            body: JSON.stringify({
              sportFavoriteId: id
            }),
          }).then(() => {
            this.$store.dispatch("getMyFavorites")
          });

        }
      },
    },
  };
</script>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Roboto+Mono&display=swap");

  .root.card-header {
    font-family: Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
    text-shadow: 0px 0px 10px rgba(0, 0, 0, 0.91);
    color: #ffffff !important;
    border-bottom: 3px solid rgb(255, 255, 255);
    background: rgb(131, 58, 180);
    background: linear-gradient(90deg, rgba(194, 6, 62, 0.527) 0%, rgba(6, 188, 194, 0.527) 50%, rgb(69, 252, 124) 100%);
  }

  .dash {
    background: rgb(174, 238, 230);
    background: -moz-radial-gradient(circle, rgba(174, 238, 230, 1) 17%, rgba(31, 121, 236, 1) 100%);
    background: -webkit-radial-gradient(circle, rgba(174, 238, 230, 1) 17%, rgba(31, 121, 236, 1) 100%);
    background: radial-gradient(circle, rgba(174, 238, 230, 1) 17%, rgba(31, 121, 236, 1) 100%);
    filter: progid:DXImageTransform.Microsoft.gradient(startColorstr="#aeeee6", endColorstr="#1f79ec", GradientType=1);
  }

  .favorite {
    background: rgb(174, 238, 230);
    background: -moz-radial-gradient(circle, rgba(174, 238, 230, 1) 17%, rgba(31, 121, 236, 1) 100%);
    background: -webkit-radial-gradient(circle, rgba(174, 238, 230, 1) 17%, rgba(31, 121, 236, 1) 100%);
    background: radial-gradient(circle, rgba(174, 238, 230, 1) 17%, rgba(31, 121, 236, 1) 100%);
    filter: progid:DXImageTransform.Microsoft.gradient(startColorstr="#aeeee6", endColorstr="#1f79ec", GradientType=1);
  }

  .dash .card {
    -webkit-box-shadow: 4px 6px 58px -11px rgba(255, 255, 255, 1);
    -moz-box-shadow: 4px 6px 58px -11px rgba(255, 255, 255, 1);
    box-shadow: 4px 6px 58px -11px rgba(255, 255, 255, 1);
  }

  .dash .card .card-header {
    font-family: Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
    text-shadow: 0px 0px 10px rgba(141, 137, 137, 0.91);
    color: #212529 !important;
  }
</style>