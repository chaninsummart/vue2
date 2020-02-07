<template>
  <div class="hello">
    <div>
        <input type="text" v-model="name" placeholder="NAME">
        <input type="text" v-model="lastname" placeholder="LASTNAME">
        <button @click="insertToContact(name,lastname)">Add</button>
    </div>

    <hr>

    <ul :key="key" v-for="(contact, key) in contact">
      <li v-if="updateKey === key">
        <input type="text" v-model="updateName" placeholder="NAME">
        <input type="text" v-model="updatelastname" placeholder="LASTNAME">
        <button @click="updateContact(upname,uplastname)">Save</button>
      </li>
      <li v-else>
        {{contact.name}} : {{contact.lastname}}
        <button @click="setUpdateContact(key, contact)">Update</button>
        <button @click="deleteContact(key)">Delete</button>
      </li>
    </ul>
  </div>
</template>
<script>

import * as firebase from 'firebase'

var config = {
  apiKey: 'AIzaSyBkai-zpyaSt65p7auAsPByEPlGghunE0Q',
  authdomain:'test-insert-form-vue.firebaseapp.com',
  databaseURL:'https://test-insert-form-vue.firebaseio.com',
  projectId:'test-insert-form-vue',
  storageBucket:'test-insert-form-vue.appspot.com',
  messaginSenderId:'1090947155880'
}
firebase.initializeApp(config)

var darabase = firebase.database()
var contactsRef = database.ref('/contacts')

export default {
  name: 'HelloWorld',
  data () {
    return {
      contacts: {},
      name: '',
      lastname: '',
      updateLastName:'',
      UpdateName:'',
      UpdateKey:'',
    }
  },
  methods: {
    insertToContact (name, lastname) {
      let data = {
        name: name,
        lastname: lastname,
      }
      contactsRef.push(data)
      this.name = ''
      this.lastname = ''
    },
    setUpdateContact (key, contact){
      this.updateKey = key
      this.updateName = contacts.name
      this.updateLastName = contacts.lastname
    },
    updateContact (name, lastname) {
      contactsRef.child(this.UpdateKey).update({
        name:name,
        lastname: lastname
      })
      this.updateKey = ''
      this.updatename = ''
      this.updateLastName = ''
    },
    deleteContact (key){ 
      contactsRef.child(key).remove()
      }
  },
  methods (){   
    contactsRef.on('value',(snapshot)=> {
      this.contacts = snapshot.val
    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
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
