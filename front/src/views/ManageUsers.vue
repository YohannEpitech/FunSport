<template>
  <div id="ManageUsers">
    <Navbar />
    <TabBar />
    <div
      class="card mt-3 mb-3 align-item-center d-flex mx-auto"
      style="width: 90%; background-color: #f4f4f4e3"
    >
      <div class="card-header d-flex justify-content-between">
        <h2>Manage Users</h2>
        <button class="btn btn-warning" @click="createUser">
          Create new user
        </button>
      </div>
      <div class="card-body overflow-auto mx-auto" style="width: 100%">
        <div
          class="row mb-2 mt-2 p-2"
          style="width: 100%; border-bottom: 1px solid silver"
          v-for="user in listUser"
          v-bind:key="user.id"
        >
          <div class="row d-flex justify-content-between" style="width: 100%">
            <h5 class="m-2">
              <span style="font-style: italic">Email : {{ user.email }}</span>
            </h5>
            <h5 class="m-2">Admin: {{ user.isAdmin }}</h5>
          </div>
          <button class="btn btn-success mr-2" @click="updateUser(user)">
            Update
          </button>
          <button class="btn btn-danger mr-2" @click="deleteUser(user._id)">
            Delete
          </button>
        </div>
      </div>
    </div>
    <div v-if="createButton == true">
      <CreateUser />
    </div>
    <div v-if="updateButton == true">
      <UpdateUser />
    </div>
  </div>
</template>

<script>
import ENV from "../../env.config";
import Navbar from "@/components/NavBar.vue";
import TabBar from "@/components/TabBar.vue";
import CreateUser from "@/components/CreateUser.vue";
import UpdateUser from "@/components/UpdateUser.vue";

/**
 * Views used to manage the users
 * @displayName Manage Users
 */
export default {
  name: "ManageUsers",
  components: {
    Navbar,
    TabBar,
    CreateUser,
    UpdateUser,
  },
  data() {
    return {
         /**
       * The list of the users
       */
      listUser: [],
       /**
       * State of the popup create
       */
      createButton: false,
      /**
       * State of the popup update
       */
      updateButton: false,
      /**
       * Paste the data for the selected user to update
       */
      userData: {},
    };
  },

  async mounted() {
    this.$store.dispatch("getUserData");
    this.$store.dispatch("getMySports");
    let list = [];
    await fetch(`http://${ENV.API_BACKEND}/users`, {
      method: "GET",
      headers: {
        authorization: "Bearer " + this.$store.state.access_token,
      },
    })
      .then((res) => res.clone().json())
      .then((json) => (list = json));
    this.listUser = list;
  },
  methods: {
    /**
     * Methods used to display the popup for updating a specific user
     * @param user user's data to update
     * @public
     */
    async updateUser(user) {
      this.userData = user;
      this.updateButton = true;
    },
    /**
     * Methods used to delete a specific user
     * @param id user id to delete
     * @public
     */
    async deleteUser(id) {
      await fetch(`http://${ENV.API_BACKEND}/users/` + id, {
        method: "DELETE",
        headers: {
          authorization: "Bearer " + this.$store.state.access_token,
        },
      });

      let list = [];
      await fetch(`http://${ENV.API_BACKEND}/users`, {
        method: "GET",
        headers: {
          authorization: "Bearer " + this.$store.state.access_token,
        },
      })
        .then((res) => res.clone().json())
        .then((json) => (list = json));
      this.listUser = list;
    },
    /**
     * Methods used to display the pop up to create a new user
     * @public
     */
    createUser() {
      this.createButton = true;
    },
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Roboto+Mono&display=swap");

  .card-header {
    font-family: Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
    text-shadow: 0px 0px 10px rgba(0, 0, 0, 0.91);
    color: #ffffff !important;
    border-bottom: 3px solid rgb(255, 255, 255);
    background: rgb(131, 58, 180);
    background: linear-gradient(90deg, rgba(194, 6, 62, 0.527) 0%, rgba(6, 188, 194, 0.527) 50%, rgb(69, 252, 124) 100%);
  }

  .card-body {
    background: rgb(174, 238, 230);
    background: -moz-radial-gradient(circle, rgba(174, 238, 230, 1) 17%, rgba(31, 121, 236, 1) 100%);
    background: -webkit-radial-gradient(circle, rgba(174, 238, 230, 1) 17%, rgba(31, 121, 236, 1) 100%);
    background: radial-gradient(circle, rgba(174, 238, 230, 1) 17%, rgba(31, 121, 236, 1) 100%);
    filter: progid:DXImageTransform.Microsoft.gradient(startColorstr="#aeeee6", endColorstr="#1f79ec", GradientType=1);
  }
</style>