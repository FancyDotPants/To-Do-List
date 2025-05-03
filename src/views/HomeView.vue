<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current" />
    <div v-if="projects.length">
      <div v-for="project in filteredProjects" :key="project.id">
        <SingleProject :project="project" @delete="handleDelete" @complete="handleComplete"/>
      </div>
    </div>
  </div>
</template>

<script>
import SingleProject from '@/components/SingleProject.vue';
import FilterNav from '@/components/FilterNav.vue';

export default {
  name: 'HomeView',
  components: {SingleProject, FilterNav},
  data() {
    return {
      projects: [],
      current : 'all'
    }
  },
  mounted() {
    fetch('http://localhost:3000/projects')
      .then(res => res.json())
      .then(data => this.projects = data)
      .catch(err => console.log(err.message))
  },
  methods: {
    handleDelete(id) {
      this.projects = this.projects.filter(item => {
      return item.id !== id
      })
      //we use the id parameter here. this function keeps everything except the one which has the id that was passed by parameter.
    },
    handleComplete(id) {
      let p = this.projects.find(item => {
        return item.id === id
      })
      p.complete = !p.complete
    }
  },
  computed: {
    filteredProjects() {
      if(this.current === 'completed') {
        return this.projects.filter(item => item.complete)
      } else if(this.current === 'ongoing') {
        return this.projects.filter(item => !item.complete)
      }else {
        return this.projects
      }
    }
  }
}
// These are the data from the db.json file incase they get deleted ;)
//   "projects": [
//{
//      "id": 1,
//      "title": "Make a Marketing Email.",
//      "details": "Lorem ipsum",
//      "complete": false
//    },
//    {
//      "id": 2,
//      "title": "Add new features to banner.",
//      "details": "Lorem ipsum",
//      "complete": true
//    },
//    {
//      "id": 3,
//      "title": "Add new items.",
//      "details": "Lorem ipsum",
//      "complete": true
//    },
//    {
//      "id": 4,
//      "title": "Fix the bugs in the forms.",
//      "details": "Lorem ipsum",
//      "complete": true
//    }
//  ]
</script>
