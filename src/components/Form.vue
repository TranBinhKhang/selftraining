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
      <span v-if="emailwrong" style="color: red; font-weight: bold;">Must be email</span>
		</div>
    <div class="row">
			<label for="DoB">Date of Birth:</label>
			<input id="DoB" v-on:click="DoBwrong = false" type="date" v-model="DoB" ref="DoB" max="2005-12-31">
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
    <button v-on:click="add">Add</button> <span v-if="isEmpty">Please input field</span>
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
  <tbody v-if='!search'>
  <tr  v-for="(user, i) in users" :key="i">
    <td v-if="!user.editing">{{user.id}}</td>
    <td v-if="!user.editing">{{user.account}}</td>
    <td v-if="!user.editing">{{user.name}}</td>
    <td v-if="!user.editing">{{user.email}}</td>
    <td v-if="!user.editing">{{user.DoB}}</td>
    <td v-if="!user.editing">{{user.role}}</td>
    <td v-if="!user.editing"><button v-on:click="edit(i)">Edit</button></td>
    <td v-if="!user.editing"><button v-on:click="remove(i)">Delete</button></td>
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
    <td v-if="user.editing"><button v-on:click="save(i)">Save</button></td>
    <td v-if="user.editing"><button v-on:click="cancel(i)">Cancel</button></td>
  </tr>
  </tbody>
  <!-- <tbody v-if='search'>
    <tr v-for="(user, i) in users" :key="i">
    <td v-if="!user.editing">1</td>
    <td v-if="!user.editing">{{user.account}}</td>
    <td v-if="!user.editing">{{user.name}}</td>
    <td v-if="!user.editing">{{user.email}}</td>
    <td v-if="!user.editing">{{user.DoB}}</td>
    <td v-if="!user.editing">{{user.role}}</td>
    <td v-if="!user.editing"><button v-on:click="edit(i)">Edit</button></td>
    <td v-if="!user.editing"><button v-on:click="remove(i)">Delete</button></td>
    <td v-if="user.editing">1</td>
    <td v-if="user.editing"><input v-model="editaccount"></td>
    <td v-if="user.editing"><input v-model="editname"></td>
    <td v-if="user.editing"><input v-model="editemail" type='email'></td>
    <td v-if="user.editing"><input v-model="editDoB" type='date'></td>
    <td v-if="user.editing"><select v-model="editrole">
			<option value="PM">PM</option>
			<option value="Dev">Dev</option>
			<option value="HR">HR</option>
</select></td>
    <td v-if="user.editing"><button v-on:click="save(i)">Save</button></td>
    <td v-if="user.editing"><button v-on:click="cancel(i)">Cancel</button></td>
  </tr>
  </tbody> -->

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
    }

}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .form  {
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
