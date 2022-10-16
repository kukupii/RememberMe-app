<template>
  <ul>
    <learning-resource
      v-for="res in storedResources"
      :key="res.id"
      :id="res.id"
      :res-title="res.title"
      :res-description="res.description"
      :res-link="res.link"
    ></learning-resource>
  </ul>
</template>

<script>
import LearningResource from "./LearningResource.vue";

export default {
  components: { LearningResource },
  props: ["newRes"],
  data() {
    return {
      storedResources: [],
    };
  },

  provide() {
    return {
      deleteResource: this.deleteResource,
    };
  },
  methods: {
    deleteResource(id) {
      const resIndex = this.storedResources.findIndex((res) => res.id === id);
      this.storedResources.splice(resIndex, 1);
    },
    addResource(data) {
      if (data) {
        this.storedResources.unshift(data);
      }
    },
    getResource() {
      fetch(
        "https://http-demo-b02d8-default-rtdb.asia-southeast1.firebasedatabase.app/resources.json"
      )
        .then((res) => {
          if (res.ok) {
            return res.json();
          }
        })
        .then((data) => {
          const resources = [];
          for (const id in data) {
            resources.push({
              id: id,
              title: data[id].title,
              description: data[id].description,
              link: data[id].url,
            });
          }
          this.storedResources = resources;
        })
        .catch((err) => {
          console.log(err);
          this.error = "Sorry, could not fetch the data. Please try later...";
        });
    },
  },
  mounted() {
    this.getResource();
    this.addResource(this.newRes);
    console.log(this.storedResources);
  },
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
  margin: auto;
  max-width: 40rem;
}
</style>
