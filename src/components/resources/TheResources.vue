<template>
  <base-card>
    <base-button @click="setTab('add-resource')" :mode="AddResourceBtn"
      >Add Resource</base-button
    >
    <base-button @click="setTab('stored-resources')" :mode="StoredResourcesBtn"
      >View Resources</base-button
    >
  </base-card>
  <keep-alive>
  <component :is="selectedTab"></component>
  </keep-alive>
</template>

<script>
import StoredResources from './StoredResources.vue';
import AddResource from './AddResource.vue';

export default {
  components: {
    StoredResources,
    AddResource,
  },
  computed: {
    AddResourceBtn() {
      return this.selectedTab == 'add-resource' ? null : 'flat';
    },
    StoredResourcesBtn() {
      return this.selectedTab == 'stored-resources' ? null : 'flat';
    },
  },
  data() {
    return {
      resources: [
        {
          id: 'google',
          name: 'Google',
          desc: 'This is the google resource',
          link: 'www.google.com',
        },
        {
          id: 'vue',
          name: 'Vue',
          desc: 'This is the vue resource',
          link: 'www.vuejs.com',
        },
      ],
      selectedTab: 'stored-resources',
    };
  },
  methods: {
    setTab(tab) {
      this.selectedTab = tab;
    },
    addResource(name, desc, link){
      const newResource = {
        id: new Date().toISOString(),
        name,
        desc,
        link
      };
      this.resources.unshift(newResource);
      this.selectedTab = 'stored-resources'
    },
    removeResource(resId){
      const resIndex = this.resources.findIndex(res => res.id == resId);
      this.resources.splice(resIndex, 1);
    }
  },
  provide() {
    return {
      resources: this.resources,
      addResource: this.addResource,
      removeResource: this.removeResource
    };
  },
};
</script>

<style scoped>
</style>