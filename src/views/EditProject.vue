<template>
  <h1>Edit Project {{id}} </h1>
  <form @submit.prevent="addProject">
      <label>Project Title</label>
      <input type="text" v-model="title">
      <label>Project Detail</label>
      <input type="text" v-model="detail">
      <button @click="updateProject">Edit Project</button>
  </form>
</template>

<script>
export default {
    props : ['id'],
    data(){
        return {
            title : '',
            detail : '',
        }
    },
    methods: {
        /** Update Project */
        updateProject(){
            fetch(`http://localhost:3000/projects/${this.id}`,{
                method : 'PATCH',
                headers : {
                    'Content-Type' : 'application/json'
                },
                body : JSON.stringify(
                    {
                        title : this.title,
                        detail : this.detail,
                    }
                )
            })
            .then(()=> this.$router.push({name : 'Home'}))
            .catch((err)=> console.log(err));
        }
    },
    mounted(){
        fetch(`http://localhost:3000/projects/${this.id}`)
        .then((response)=> response.json())
        .then((response)=>{
            this.title = response.title;
            this.detail = response.detail;
        })
        .catch((err)=>console.log(err));
    }
}
</script>

<style>

</style>