<template>
  <div class="users">
    <h1>Retrieve all Users</h1>

    <h3>Just some database interaction...</h3>

    <button @click="retrieveAllUserAPI()">Retrieve users data from database</button>

    <ul id="allusers">
       <li v-for="(value, key) in FakeAllUser ">
            {{ key }} : {{ value }}
        </li>
    </ul>

    All first names:
    <ul id="allusers3">
       <li v-for="(value, key) in FakeFirstNames ">
             {{ value }}
        </li>
    </ul>



    



    

  </div>
</template>

<script>
import api from "./backend-api";

export default {
  name: "users",

  data() {
    return {
      response: [],
      errors: [],
      user: {
        lastName: "",
        firstName: "",
        id: 0
      },
      showResponse: false,
      retrievedUser: {},
      showRetrievedUser: false,
      showFakeAllUser: false,
      FakeAllUserSet: [
        { nameS: "Foo" },
        { nameS: "Bar" },
        { nameS: "Zuu" },
        { nameS: "Fii" }
      ],
      FakeAllUser: {},
      FakeFirstNames: {}
    };
  },
  methods: {
    // Fetches posts when the component is created.
    createNewUser() {
      api
        .createUser(this.user.firstName, this.user.lastName)
        .then(response => {
          // JSON responses are automatically parsed.
          this.response = response.data;
          this.user.id = response.data;
          console.log("Created new User with Id " + response.data);
          this.showResponse = true;
        })
        .catch(e => {
          this.errors.push(e);
        });
    },
    retrieveAllUser() {
      this.showFakeAllUser = true;
      this.FakeAllUser = this.FakeAllUserSet;
    },

    retrieveAllUserAPI() {
      api
        .getAllUser()
        .then(response => {
          // response.data.array.forEach(element => {
          //   console.log(response.data);
          // });
          console.log(response.data.length);
          let temp_response = response.data;

          //this.FakeAllUser = response.data[5];

          this.showFakeAllUser = true;
          console.log(temp_response);

          let firstName_list = [];
          for (let i = 0; i < temp_response.length; i += 1) {
            firstName_list.push(temp_response[i].lastName);
          }

          this.FakeAllUser = temp_response;
          this.FakeFirstNames = firstName_list;

          //console.log(response.data.length);
        })
        .catch(e => {
          this.errors.push(e);
          console.log(e);
        });
    },

    retrieveUser() {
      api
        .getUser(this.user.id)
        .then(response => {
          // JSON responses are automatically parsed.
          this.retrievedUser = response.data;
          this.showRetrievedUser = true;
        })
        .catch(e => {
          this.errors.push(e);
        });
    }
  }
};
</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
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
}
</style>
