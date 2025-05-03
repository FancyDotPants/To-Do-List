<template>
  <nav class="filter-nav">
    <button @click="updateFilter('all')" :class="{active: current === 'all'}">View All</button>
    <button @click="updateFilter('completed')" :class="{active: current === 'completed'}">Completed</button>
    <button @click="updateFilter('ongoing')" :class="{active: current === 'ongoing'}">Ongoing</button>
  </nav>
</template>

<script>
export default {
    props: ['current'],
    methods: {
        updateFilter(by) {
            this.$emit('filterChange', by)
        }
    }
}
// I created the filterChange event for the HomeView.vue File usage so whenever the user uses that component, the functions run and it also sends the current props.
// I have created a dynamic class for the buttons for the color change of the buttons when they're clicked.
// The current is the props that gets sent from the HomeView.vue File and must match the conditions which i declared.
</script>

<style>
    .filter-nav button {
        background: none;
        border: none;
        color: #bbb;
        outline: none;
        font-size: 12px;
        text-transform: uppercase;
        margin-right: 10px;
        letter-spacing: 1px;
        font-weight: bold;
        cursor: pointer;
    }
    .filter-nav button.active {
        color: #555;
    }
</style>