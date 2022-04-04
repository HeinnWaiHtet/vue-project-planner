<template>
  <div class="home">
    <h1>Home</h1>
    <FilterNav @filterValue="current= $event" :current='current'>

    </FilterNav>
    <div v-for="project in filterProject" :key="project.id">
      <SingleProject
        :project='project'
        @delete='deleteProject'
        @complete="completeProject"></SingleProject>
    </div>
  </div>
</template>

<script>


import FilterNav from '../components/FilterNav'
import SingleProject from '../components/SingleProject'
export default {
  name: 'Home',
  mounted(){
    this.getAllProjects();
  },
  components: {
    FilterNav,
    SingleProject,
  },
  data(){
    return{
      projects : [],
      current : 'all',
    }
  },
  computed:{
    /** Filter Project By Status */
    filterProject(){
      if(this.current === 'ongoing'){
        return this.projects.filter((project) => !project.complete);
      }
      else if(this.current === 'complete'){
        return this.projects.filter((project) => project.complete);
      }
      return this.projects;
    }
  },
  methods:{
    /** Get List Of Projects */
    getAllProjects(){
      fetch('http://localhost:3000/projects').
      then((response)=> response.json()).
      then((response)=>{
        this.projects = response;
      }).
      catch((err)=>console.log(err));
    },

    /** Delete Project By Request Id */
    deleteProject(requestId){
      this.projects = this.projects.filter((project) => project.id != requestId);
    },

    /** Change Project Status By Request Id */
    completeProject(id){
      let currentProject = this.projects.find((project) => project.id ==id);
      currentProject.complete = !currentProject.complete;
    }
  }
}
</script>
