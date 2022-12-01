
<script setup>
import {ref} from 'vue'
let users=ref([])
let name = ref("")
let email=ref("")
let job=ref("")
let isUsersHave=false 
let isUserUpdated=ref(false)
let isUserUpdatedId=ref(null)


/// Add User 
const addUser=function(e){
  e.preventDefault()
  if(name.value==="" || email.value==="" || job.value===""){
    alert("Maydonlarni toldiring.")
    return 0 
  }

  else if(isUserUpdated.value===true){
    let findUser=users.value.find(user=>user.id==isUserUpdatedId.value)
    findUser.name=name.value 
    findUser.email=email.value
    findUser.job=job.value
      name.value=""
      email.value=""
      job.value=""
    isUserUpdated.value=false
    isUserUpdatedId.value=null
   
  }
  


 else{
  let newUser={
    id:users.value.length+1,
    name:name.value,
    email:email.value,
    job:job.value
  }

  users.value.push(newUser)

  name.value=""
  email.value=""
  job.value=""
  
 }


}

/// delete user 

function deleteUser(id){
  users.value=users.value.filter(user=>user.id!=id)

}


/// update User

function updateUser(id){
  let findUser= users.value.find(user=>user.id==id)
    name.value=findUser.name 
    email.value=findUser.email
    job.value=findUser.job 

    isUserUpdated.value=true
    isUserUpdatedId.value=findUser.id  

}
function haveUsers(){
  if(users.value.length){
    return ` Userlar soni: ${users.value.length}`
  }
  else{
    return `Hozircha userlar mavjud emas`
  }
}

function checkUpdatedButton(){
  if(isUserUpdated.value===true){
    return "Update User"
  }
  else{
    return "Add User"
  }

}
</script>
 
<template>

   <div class="container">
      <div class="row mt-5">
          <h2 class="text-center" >{{haveUsers()}}</h2>
        <form  class="col-md-6 offset-3" @submit="addUser">
          <input type="text" class="form-control" v-model="name">
          <input type="text" class="form-control my-2" v-model="email">
          <input type="text" class="form-control" v-model="job">
           <button type="submit" class="btn btn-success mt-3">{{checkUpdatedButton()}}</button>
        </form>
        <div>
          <ul>
           <table class="table table-bordered mt-3 usertable">
            <thead>
              <tr>
                <th>id:</th>
                <th>name:</th>
                <th>email:</th>
                <th>job:</th>
                <th>update</th>
                <th>delete</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="user in users" :key="user.id">
                <td>{{user.id}}</td>
                <td>{{user.name}}</td>
                <td>{{user.email}}</td>
                <td>{{user.job}}</td>
                 <td><button class="btn btn-primary" @click="updateUser(user.id)"> update</button></td>
                 <td><button class="btn btn-danger" @click="deleteUser(user.id)">delete</button></td>
              </tr>
            </tbody>
           </table>
          </ul>
        </div>
      
      </div>

   </div>
</template>

<style>
.usertable{
  height: 20px;
  width: 400px!important;
  overflow: scroll!important
}
</style>

