<template>
  <div class="project" :class='{complete:project.complete}'>
      <div class="flex">
          <div>
              <h3 @click="showDetail = !showDetail"> {{project.title}} </h3>
          </div>
          <div>
              <span class="material-icons" @click="deleteProject">
                  delete
              </span>
              <router-link :to="{ name : 'EditProject', params : {id:project.id}}">
                    <span class="material-icons">
                       edit
                    </span>
              </router-link>
              <span class="material-icons" @click="complete">
                  done
              </span>
        </div>
      </div>
      <p v-if="showDetail"> {{project.detail}} </p>
  </div>
</template>

<script>
export default {
    props:['project'],
    data(){
        return{
            showDetail : false,
            url : 'http://localhost:3000/projects/',
        }
    },
    methods:{
        /** Delete Project */
        deleteProject(){
            let requestUrl = this.url + this.project.id;
            fetch(requestUrl, {method : 'DELETE'})
            .then(()=>{
                this.$emit('delete', this.project.id);
            })
            .catch((err) => {
                console.log(err);
            });
        },
        /** Change Project To Complete State */
        complete(){
            let requestUrl = this.url + this.project.id;
            fetch(requestUrl,{
                method : 'PATCH',
                headers : {
                    'Content-Type': 'application/json'
                },
                body:JSON.stringify(
                    {
                        complete : !this.project.complete
                    }
                )
            })
            .then(()=>{
                this.$emit('complete', this.project.id);
            })
            .catch((err)=> console.log(err));
        }
    }
}
</script>

<style scoped>
.project{
    padding:20px;
    background-color: #f2f2f2;
    margin: 10px;
    border-left: 6px solid crimson;
    border-radius: 20px;
}

h3{
    color: indigo;
    cursor: pointer;
}

.flex{
    display: flex;
    justify-content: space-between;
    align-items: center;
}
span{
    margin-left: 10px;
}
span:hover{
    color: #777;
    cursor: pointer;
}

.complete{
    border-left-color: green;
}
</style>