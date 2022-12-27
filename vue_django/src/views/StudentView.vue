<template>
    <div class="container-fluid"> 
        <h2 class="alert alert-danger mt-2">Django 4 And Vue JS 3 CRUD Application</h2>

        <div class="row">

            


            <div class="col-md-7">


                <h2 class="alert alert-success">List of Students</h2>

                <table class="table table-bordered mt-4">
  <thead>
    <tr>
      <th scope="col">Name</th>
      <th scope="col">Course</th>
      <th scope="col">Email</th>
      <th scope="col">Gender</th>
      <th scope="col">Action</th>
    </tr>
  </thead>
  <tbody>
    <tr v-for="student in students">
      <td>{{student.name}}</td>
      <td>{{student.course}}</td>
      <td>{{student.email}}</td>
      <td>{{student.gender}}</td>
      <td>
        <a href="#" class="edit" title="" ><button class="btn btn-warning btn-sm" @click="editBtn(student.id)">Edit</button></a>
        <a href="#" class="edit ml-1" title="" ><button  class="btn btn-danger btn-sm" @click="deleteStudent(student.id)">Delete</button></a>


      </td>
    </tr>
  </tbody>
</table>

            </div>


            <div class="col-md-5">



                  <!-- There is a current student to be edited -->

                <div v-if="Object.keys(this.currentStudent).length !==0">
                

                    <h2 class="alert alert-warning">Edit Student Details</h2>

                    <form @submit.prevent="updateStudent(currentStudent.id)">

<div class="row">

    <div class="col">
        <div class="form-group">
<label class="form-label float-left ml-2">Name</label>
<input type="text" class="form-control" v-model="currentStudent.name">
</div>
    </div>

    <div class="col">
        <div class="form-group">
<label class="form-label float-left ml-2">Email</label>
<input type="email" class="form-control" v-model="currentStudent.email">
</div>
    </div>
</div>



<div class="row">
<div class="col">
<div class="form-group">
<label class="form-label float-left ml-2">Course</label>
<input type="text" class="form-control" v-model="currentStudent.course">
</div>
</div>
<div class="col">

<div class="form-group">
<label class="form-label float-left ml-2">Gender</label>
<input type="text" class="form-control" v-model="currentStudent.gender">
</div>
</div>

</div>
<button type="submit" class="btn btn-success float-left ml-2">Update</button>
</form>

                </div>




                <!-- There is no current student to be edited -->

                <div v-else>
                    <h2 class="alert alert-info">Create A New Student</h2>
                    <form @submit.prevent="saveStudent()">

<div class="row">

    <div class="col">
        <div class="form-group">
<label class="form-label float-left ml-2">Name</label>
<input type="text" class="form-control" v-model="student.name">
</div>
    </div>

    <div class="col">
        <div class="form-group">
<label class="form-label float-left ml-2">Email</label>
<input type="email" class="form-control" v-model="student.email">
</div>
    </div>
</div>



<div class="row">
<div class="col">
<div class="form-group">
<label class="form-label float-left ml-2">Course</label>
<input type="text" class="form-control" v-model="student.course">
</div>
</div>
<div class="col">

<div class="form-group">
<label class="form-label float-left ml-2">Gender</label>
<input type="text" class="form-control" v-model="student.gender">
</div>
</div>

</div>
<button type="submit" class="btn btn-primary float-left ml-2">Save</button>
</form>
                </div>
            </div>
        </div>
    </div>

</template>


<script>
import axios from 'axios'

export default{

    data(){
        return{
        students: [],
        currentStudent: {},
        'api': 'http://127.0.0.1:8000/api',
        'student': {
            'name':'',
            'email':'',
            'course':'',
            'gender':'',
        }
        }
    },

    mounted(){
     console.log('DOM is rendered')
     console.log(Object.keys(this.currentStudent).length)
    },

    created(){
     console.log('DOM is created')
     this.getStudents()
    },


    methods: {

    getStudents(){
    axios.get(this.api + '/students/').then(
        response =>{
            console.log(response.data)
            this.students = response.data;

        }
    ).catch(error =>{
        console.log(error)
    })

},

saveStudent(){
    axios.post(this.api + '/students/', this.student).then(
        response =>{
            console.log(response.data)
            this.getStudents()
            this.student = {}
        }
    ).catch(error =>{
        console.log(error)
    })

},

editBtn(id){
    console.log(id)
    this.students.map(student => {
        if(student.id === id){
            console.log(student.name)
            this.currentStudent = {'id':student.id, 'name':student.name, 'email':student.email, 'course':student.course, 'gender':student.gender}
        }
        
    })

},

updateStudent(id){

    axios.put(this.api + `/students/${id}/`, this.currentStudent).then(
        response =>{
            console.log(response.data)
            this.getStudents()
            this.currentStudent = {}
        }
    ).catch(error =>{
        console.log(error)
    })


},



deleteStudent(id){

axios.delete(this.api + `/students/${id}/`, id).then(
    response =>{
        console.log(response.data)
        this.getStudents()
    }
).catch(error =>{
    console.log(error)
})


},



}

}
</script>