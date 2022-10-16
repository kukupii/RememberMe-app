<template>
  <base-card>
    <base-button
      @click="setSelectedTab('stored-resource')"
      :mode="storedResourceMode"
      class="nav"
      >Stored Resources</base-button
    >
    <base-button
      @click="setSelectedTab('add-resource')"
      :mode="addResourceMode"
      class="nav"
      >Add Resource</base-button
    >
  </base-card>

  <p v-if="error">{{ error }}</p>
  <keep-alive>
    <component
      :is="selectedTab"
      @changeTab="changeAfterSubmit"
      :newRes="receivedData"
    ></component>
  </keep-alive>
</template>

<script>
import storedResource from "./StoredResource.vue";
import addResource from "./AddResource.vue";
export default {
  components: { storedResource, addResource },
  data() {
    return {
      selectedTab: "stored-resource",
      receivedData: {},
      error: null,
    };
  },

  provide() {
    return {
      // addResource: this.addResource,
      // deleteResource: this.deleteResource,
    };
  },
  methods: {
    // getResource() {
    //   fetch(
    //     "https://http-demo-b02d8-default-rtdb.asia-southeast1.firebasedatabase.app/resources.json"
    //   )
    //     .then((res) => {
    //       if (res.ok) {
    //         return res.json();
    //       }
    //     })
    //     .then((data) => {
    //       const resources = [];
    //       for (const id in data) {
    //         resources.push({
    //           id: id,
    //           title: data[id].title,
    //           description: data[id].description,
    //           link: data[id].url,
    //         });
    //       }
    //       console.log(resources);
    //       this.storedResources = resources;
    //       console.log(this.storedResources);
    //     })
    //     .catch((err) => {
    //       console.log(err);
    //       this.error = "Sorry, could not fetch the data. Please try later...";
    //     });
    // },
    setSelectedTab(tab) {
      this.selectedTab = tab;
    },

    changeAfterSubmit(title, description, url) {
      const receivedRes = {
        id: new Date().toISOString(),
        title: title,
        description: description,
        link: url,
      };
      this.receivedData = receivedRes;
      this.selectedTab = "stored-resource";
    },
    // addResource(title, description, url) {
    //   const receivedRes = {
    //     id: new Date().toISOString(),
    //     title: title,
    //     description: description,
    //     link: url,
    //   };
    //   this.storedResources.unshift(receivedRes);
    //   this.selectedTab = "stored-resource";
    // },
  },
  computed: {
    storedResourceMode() {
      return this.selectedTab === "stored-resource" ? "flat" : null;
    },
    addResourceMode() {
      return this.selectedTab === "add-resource" ? "flat" : null;
    },
  },
};
</script>
