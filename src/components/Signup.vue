<template>
<div>
    <p v-if="error.length>0">
        <b>Please correct the following errors:</b>
        <ul>
            <li v-for="e in error" v-bind:key="e.id">
                {{e}}
            </li>
        </ul>
    </p>
    <b-form @submit="signup">
        <b-form-input width: auto type="text" placeholder="Enter name" v-model="users.name"/> <br />
        <b-form-input type="text" placeholder="Enter department" v-model="users.department"/> <br /> 
        <b-form-input type="text" placeholder="Enter semester" v-model="users.semester"/> <br /> 
        <b-button type="submit">Signup </b-button>
      
    </b-form>
    <br />
    
     <form @submit="signup">
        <input type="text" placeholder="Enter id" v-model="updated_user.id"/> <br /> <br />

        <input type="text" placeholder="Enter name" v-model="updated_user.name"/> <br /> <br />
        <input type="text" placeholder="Enter department" v-model="updated_user.department"/> <br /> <br />
        <input type="text" placeholder="Enter semester" v-model="updated_user.semester"/> <br /> <br />
        <b-button v-on:click="updateRecord">Update </b-button>
      
    </form>
    <p>Get Data</p>
    <b-button v-on:click="getList">Get data</b-button>
    <b-table striped hover :items="userArray">
    </b-table>
   
    <!-- <ul v-if="userArray.length>0">
      <li>Name Department Semester</li>
      <li v-for="user in userArray" :key="user.id">
        {{user.name}} {{user.department}} {{user.semester}}
        <button v-on:click="deleteRecord(user.id)">Delete</button>
      
      </li>
    </ul> -->

    <div>
  <b-button>Button</b-button>
  <b-button variant="danger">Button</b-button>
  <b-button variant="success">Button</b-button>
  <b-button variant="outline-primary">Button</b-button>
</div>
</div>
</template>
<script>
export default {
  name: "Signup",
  data() {
    return {
      error: [],
      api: "http://localhost:8080//students//",
      userArray:[],
      users: {
        name: null,
        department: null,
        semester: null,
      },
      updated_user: {
        id:null,
        name: null,
        department: null,
        semester: null,
      },
    };
  },
  methods: {
    updateRecord(){
      this.updated_user.id = parseInt(this.updated_user.id);
      this.axios.put(this.api+this.updated_user.id, this.updated_user);
    },
    deleteRecord(id){
      this.axios.delete(this.api+id).then(() => {
        console.warn('working...');
        this.getList();
      })
    },
    getList(){
      this.axios.get(this.api).then((response) => {
        console.log(response.data);
        this.userArray = response.data;
      })
    },
    signup(e) {
      this.error = [];

      if (this.users.name && this.users.department && this.users.semester) {
        console.warn("no error");
      }
      if (!this.users.name) {
        this.error.push("username required.");
      }
      if (!this.users.department) {
        this.error.push("department required.");
      }
      if (!this.users.semester) {
        this.error.push("semester required.");
      }
      this.users.semester=parseInt(this.users.semester);
     
      // alert(JSON.stringify(this.users));

      this.axios.post(this.api, this.users);

      console.warn("Hi", this.error);
      e.preventDefault();
    },
  },
};
</script>
