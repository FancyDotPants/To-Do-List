<template>
    <div class="project" :class="{complete: project.complete}">
        <div class="action">
            <h3 @click="showDetails = !showDetails">{{project.title}}</h3>
            <div class="icons">
                <span class="material-symbols-outlined" @click="deleteProject">delete</span>
                <router-link :to="{name: 'EditProject', params: {id: project.id}}">
                    <span class="material-symbols-outlined">edit</span>
                </router-link>
                <span class="material-symbols-outlined" @click="changeComplete">done</span>
            </div>
        </div>
        <div v-if="showDetails" class="details>">
            <p>{{project.details}}</p>
        </div>
    </div>
</template>

<script>
export default {
    props: ['project'],
    data() {
        return {
            showDetails: false,
            uri: 'http://localhost:3000/projects/' + this.project.id
            // I created this uri data for easy access to the endpoint.
        }
    },
    methods: {
        deleteProject() {
            fetch(this.uri, {method: 'DELETE'})
                .then(() => this.$emit('delete', this.project.id))
                .catch(err => console.log(err.message))
            // we use the DELETE request method to delete the data from "db.json" file.
        },
        changeComplete() {
            fetch(this.uri, {
                 method: 'PATCH',
                 headers: { 'Content-Type': 'application/json'},
                 body: JSON.stringify({complete: !this.project.complete})})
                    .then(() => this.$emit('complete', this.project.id))
                    .catch(err => console.log(err.message))
            // we use the PATCH request method to change only one property in the "db.json" file.
        }
    }
}
</script>

<style>
    .project {
        margin: 20px auto;
        background: white;
        padding: 10px 20px;
        border-radius: 4px;
        box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.5);
        border-left: 4px solid #e90074;
    }

    h3 {
        cursor: pointer;
    }

    .action {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .material-symbols-outlined {
        font-size: 30px;
        margin-left: 20px;
        color: #bbb;
        cursor: pointer;
    }
    .material-symbols-outlined:hover {
        color: #777;
    }
    .project.complete {
        border-left: 4px solid #00ce89;
    }
</style>