<template>
   <h1>Form</h1>
   <div class="form">
		<div class="row">
			<label for="account">Account:</label>
			<input id="account" type="text" v-model="account">
		</div>
		<div class="row">
			<label for="name">Full name:</label>
			<input id="name" type="text" v-model="name">
		</div>
    <div class="row">
			<label for="email">Email:</label>
			<input id="email" v-on:click="emailwrong = false" type="email" v-model="email" ref="email">
      <div v-if="emailwrong" class="tooltip">Must be email</div>
		</div>
    <div class="row">
			<label for="DoB">Date of Birth:</label>
			<input id="DoB" v-on:click="DoBwrong = false" type="date" v-model="DoB" ref="DoB" max="2005-12-31">
      <div v-if="DoBwrong" class="tooltip">Must be email</div>
		</div>
    <div class="row">
<label for="role">Role:</label>
<select id="role" v-model="role">
			<option value="PM">PM</option>
			<option value="Dev">Dev</option>
			<option value="HR">HR</option>
</select>
</div>
    <div class='row'>
          <button v-on:click="add">Add</button>
    <div v-if="isEmpty" class="tooltip">Must be email</div>
    </div>
    <input v-model="search">
    <button @click="test">test</button>
</div>

  <table>
  <thead>
  <tr>
    <th>#</th>
    <th><a href="#" v-on:click="sortAccount">Account</a></th>
    <th><a href="#" v-on:click="sortName">Full Name</a></th>
    <th><a href="#" v-on:click="sortEmail">Email</a></th>
    <th><a href="#" v-on:click="sortDoB">DoB</a></th>
    <th><a href="#" v-on:click="sortRole">Role</a></th>
    <th></th>
    <th></th>
  </tr>
  </thead>
  <tbody>
  <tr  v-for="(user, i) in userList" :key="i">
    <td v-if="!user.editing">{{user.id}}</td>
    <td v-if="!user.editing">{{user.account}}</td>
    <td v-if="!user.editing">{{user.name}}</td>
    <td v-if="!user.editing">{{user.email}}</td>
    <td v-if="!user.editing">{{user.DoB}}</td>
    <td v-if="!user.editing">{{user.role}}</td>
    <td v-if="!user.editing"><button v-on:click="edit(i)">Edit</button><button v-on:click="remove(i)">Delete</button></td>
    <td v-if="user.editing">{{user.id}}</td>
    <td v-if="user.editing"><input v-model="editaccount"></td>
    <td v-if="user.editing"><input v-model="editname"></td>
    <td v-if="user.editing"><input v-model="editemail" type='email'></td>
    <td v-if="user.editing"><input v-model="editDoB" type='date'></td>
    <td v-if="user.editing"><select v-model="editrole">
			<option value="PM">PM</option>
			<option value="Dev">Dev</option>
			<option value="HR">HR</option>
    </select></td>
    <td v-if="user.editing"><button v-on:click="save(i)">Save</button><button v-on:click="cancel(i)">Cancel</button></td>
  </tr>
  </tbody>

  </table>

</template>

<script>

export default {
  name: 'Form',
  data () {
        return {
        account: '',
        name: '',
        email: '',
        DoB: '',
        role: '',
        users: [],
        emailwrong: false,
        DoBwrong: false,
        isEmpty: false,
        editaccount: '',
        editname: '',
        editemail: '',
        editDoB: '',
        editrole: '',
        ascending: {
          account: true,
          name: true,
          email: true,
          DoB: true,
          role: true
        },
        search: ''
    }
	},
  methods: {
    add() {
        const newUser = {
          account: this.account,
          name: this.name,
          email: this.email,
          DoB: this.DoB,
          role: this.role,
          editing: false,
        }
        if(!this.$refs.email.checkValidity()) this.emailwrong = true 
        else this.emailwrong = false;
        if(!this.$refs.DoB.checkValidity()) this.DoBwrong = true
        else this.DoBwrong = false;
        if (this.account === '', this.name=== '', this.email === '', this.DoB === '', this.role === '') this.isEmpty = true;
        else this.isEmpty = false;
        if (this.emailwrong || this.DoBwrong || this.isEmpty ) return;
        this.users.push(newUser);
        const index = this.users.indexOf(newUser);
        this.users[index].id = index + 1;
    },
    edit(i) {
      this.editaccount = this.users[i].account;
      this.editname = this.users[i].name;
      this.editemail = this.users[i].email;
      this.editDoB = this.users[i].DoB;
      this.editrole = this.users[i].role;
      this.users[i].editing = true;
      console.log(i);
    },
    remove (i) {
        if (confirm('Are you sure you want to delete?') == true) {
        this.users.splice(i, 1)
        } else {
        return;
        }
    },
    cancel (i) {
      this.users[i].editing = false;
    },
    save(i) {
      this.users[i].account = this.editaccount;
      this.users[i].name = this.editname;
      this.users[i].email = this.editemail;
      this.users[i].DoB = this.editDoB;
      this.users[i].role = this.editrole;
      this.users[i].editing = false;
    },
    sortAccount() { //sort() can't take parameter except a, b so have to do this
      this.ascending.account = !this.ascending.account;
      Object.keys(this.ascending).forEach(i => i !== 'account' ? this.ascending[i] = true : null); //reset the object
      if (this.ascending.account) this.users.sort((a, b) => a.account > b.account ? 1 : -1);
      else this.users.sort((a, b) => a.account > b.account ? -1 : 1);
    },
    sortName() {
      this.ascending.name = !this.ascending.name;
      Object.keys(this.ascending).forEach(i => i !== 'name' ? this.ascending[i] = true : null);
      if (this.ascending.name) this.users.sort((a, b) => a.name > b.name ? 1 : -1);
      else this.users.sort((a, b) => a.name > b.name ? -1 : 1);
      },
    sortEmail() {
      this.ascending.email = !this.ascending.email;
      Object.keys(this.ascending).forEach(i => i !== 'email' ? this.ascending[i] = true : null);
      if (this.ascending.email) this.users.sort((a, b) => a.email > b.email ? 1 : -1);
      else this.users.sort((a, b) => a.email > b.email ? -1 : 1);
    },
    sortDoB() {
      this.ascending.DoB = !this.ascending.DoB;
      Object.keys(this.ascending).forEach(i => i !== 'DoB' ? this.ascending[i] = true : null);
      if (this.ascending.DoB) this.users.sort((a, b) => a.DoB > b.DoB ? 1 : -1);
      else this.users.sort((a, b) => a.DoB > b.DoB ? -1 : 1);
    },
    sortRole() {
      this.ascending.role = !this.ascending.role;
      Object.keys(this.ascending).forEach(i => i !== 'role' ? this.ascending[i] = true : null);
      if (this.ascending.role) this.users.sort((a, b) => a.role > b.role ? 1 : -1);
      else this.users.sort((a, b) => a.role > b.role ? -1 : 1);
    },
    test() {
      console.log(this.users);
    }
  },
    computed: {
    userList: function () {
          const searchResult = this.users.filter(user => Object.values(user).includes(this.search));
          if (this.search) return searchResult;
          else return this.users;
    }
  },
  mounted() {
      fetch('https://api.jsonbin.io/b/61bf42d6b8fdb92a527ba3cd/3')
      .then(response => response.json())
      .then(json => {console.log(json); this.users = json})
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .form  {
    position: relative;
  }
	 .row { 
    display: table-row;
    position: relative;
  }
	.form label { 
    display: table-cell; 
  }
	.form input { 
    display: table-cell; 
  }
  
  .tooltip {
    text-align: center;
    padding: 5px;
    border-style: solid;
    border: red;
    border-width: 15px;
    z-index: 100;
    background-color: orange;
    color: white;
    left: 101%;
    top: 5%;
    width: 250px;
    position: absolute;
    border-radius: 6px;
}

table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

td, th {
  border: 1px solid #dddddd;
  text-align: left;
}


</style>
