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
			<input id="email" type="email" v-model="email">
      <p v-if="emailwrong">Must be email</p>
		</div>
    <div class="row">
			<label for="DoB">Date of Birth:</label>
			<input id="DoB" type="datetime-local" v-model="DoB">
      <p v-if="DoBwrong">Must be in date/time</p>
		</div>
    <div class="row">
<label for="role">Role:</label>
<select id="role" v-model="role">
			<option value="PM">PM</option>
			<option value="Dev">Dev</option>
			<option value="HR">HR</option>
</select>
</div>
    <button v-on:click="add">Add</button>
</div>

  <table>
  <thead>
  <tr>
    <th>#</th>
    <th>Account</th>
    <th>Full Name</th>
    <th>Email</th>
    <th>DoB</th>
    <th>Role</th>
    <th></th>
    <th></th>
  </tr>
  </thead>
  <tbody v-if="edituser">
  <tr  v-for="(user, i) in users" :key="i">
    <td>1</td>
    <td><input v-model="editaccount"></td>
    <td><input v-model="editname"></td>
    <td><input v-model="editemail"></td>
    <td><input v-model="editDoB"></td>
    <td><input v-model="editRole"></td>
    <td><button v-on:click="save(i)">Save</button></td>
    <td><button v-on:click="cancel(i)">Cancel</button></td>
  </tr>
  </tbody>
  <tbody v-else>
  <tr  v-for="(user, i) in users" :key="i">
    <td>1</td>
    <td>{{account}}</td>
    <td>{{name}}</td>
    <td>{{email}}</td>
    <td>{{DoB}}</td>
    <td>{{role}}</td>
    <td><button v-on:click="edit(i)">Edit</button></td>
    <td><button v-on:click="remove(i)">Delete</button></td>
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
		test: 1,
    edituser: false,
		users: [],
    emailwrong: false,
    DoBwrong: false

    }
	},
  methods: {
    add() {
        const newUser = {
          account: this.account,
          name: this.name,
          email: this.email,
          DoB: this.DoB,
          role: this.role
        }
        if (!this.email.match("/^[a-zA-Z0-9._-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,6}$/")) this.emailwrong = true;
        if (!this.email.match("/^(0?[1-9]|[12][0-9]|3[01])[\/\-](0?[1-9]|1[012])[\/\-]\d{4}$")) this.DoBwrong = true;
        if (this.emailwrong || this.DoBwrong ) return;
        this.users.push(newUser);
        console.log(this.users[0]);
    },
    edit(i) {
      this.editaccount = this.users[i].account;
      this.editname = this.users[i].name;
      this.editemail = this.users[i].email;
      this.editDoB = this.users[i].DoB;
      this.editrole = this.users[i].role;
      this.edituser = true;
      console.log(i);
    },
    remove (i) {
      this.users.splice(i, 1)
    }
}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .form	form  {
    display: table;
  }
	div .row { 
    display: table-row;  
  }
	.form label { 
    display: table-cell; 
  }
	.form input { 
    display: table-cell; 
  }
</style>
