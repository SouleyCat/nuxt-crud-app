<template>
  <div class="container">
      <h2 class="text-center mt-5 mb-3">Projet CRUD NUXT</h2>
      <div class="card">
          <div class="card-header text-end">
              <NuxtLink to="/create"
                  class="btn btn-outline-primary"
                  >Nouveau Projet
              </NuxtLink>
          </div>
          <div class="card-body">
         
              <table class="table table-bordered">
                  <thead>
                      <tr>
                          <th>Nom</th>
                          <th>Description</th>
                          <th width="300px">Actions</th>
                      </tr>
                  </thead>
                  <tbody>
                         
                      <tr v-for="project in projects" :key="project.id">
                          <td>{{project.name}}</td>
                          <td>{{project.description}}</td>
                          <td>
                              <NuxtLink :to="`/show/${project.id}`" class="btn btn-outline-info mx-1">Voir</NuxtLink>
                              <NuxtLink :to="`/edit/${project.id}`" class="btn btn-outline-success mx-1">Modifier</NuxtLink>
                              <button 
                                  @click="handleDelete(project.id)"
                                  className="btn btn-outline-danger mx-1">
                                  Supprimer
                              </button>
                          </td>
                      </tr>
                             
                  </tbody>
              </table>
          </div>
      </div>
  </div>
</template>
  
<script>
 
 
import { deleteProject, getProjects } from '~/services/projectService'
import Swal from 'sweetalert2'
 
export default {
 
  data() {
    return {
      projects:[]
    };
  },
   
  created(){
    this.fetchProjectList()
  },
 
  methods:{
    fetchProjectList() {       
      getProjects()
        .then(response => {
            this.projects = response.data;
            return response
        })
        .catch(error => {
          return error
      });
 
    },
 
    handleDelete(id){
      Swal.fire({
          title: 'Confirmation',
          text: "Votre projet sera supprimé!",
          icon: 'warning',
          showCancelButton: true,
          confirmButtonColor: '#3085d6',
          cancelButtonColor: '#d33',
          confirmButtonText: 'Oui!'
        }).then((result) => {
        if (result.isConfirmed) {
          deleteProject(id)
            .then( response => {
                Swal.fire({
                    icon: 'success',
                    title: 'Projet supprimé!',
                    showConfirmButton: false,
                    timer: 1500
                })
                this.fetchProjectList();
                return response
            })
            .catch(error => {
                Swal.fire({
                      icon: 'error',
                    title: 'An Error Occured!',
                    showConfirmButton: false,
                    timer: 1500
                })
                return error
            });
        }
      })
    }
   
 
  }
};
</script>