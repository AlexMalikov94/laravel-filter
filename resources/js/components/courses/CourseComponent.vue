<template>
<div class="container">
<div class="row">
    <div class="col-sm-3">
       <filters endpoints="/api/courses/filters"></filters>
    </div>

    <div class="col-sm-9">
      <div class="card">
        <div class="card-body">
            <template v-if="courses.length">
              <course v-for="course in courses" :course="course" :key="course.id">
              
                </course>
                     <pagination :meta="meta" v-on:pagination="getCourses"></pagination>
            </template>
            <template v-else>
                Not Results found
            </template>
            
       
        </div>
        </div>
    </div>
    
</div>
</div>


</template>
<script>
//import Course from './partials/Course.vue'
export default {
 
    data() {
        return {
            courses: [],
            meta: {}
        }
    },
    mounted() {
       this.getCourses()
    },
    watch: {
       '$route.query' : {
           handler (query) {
             this.getCourses(1, query)
           },

           deep: true
       }
    },
    methods: {
        getCourses(page = 1, query = {}) {
         axios.get('/api/courses', {
             params: {
                 page,
                 ...query
             }
         }).then((response) => {
            this.courses = response.data.data
            this.meta = response.data.meta
        })
        }
    }
}
</script>