<template>
    <div>
        <div class="search" v-on:click="showFullSearch"  v-if="!isShow">
            <img src=".././assets/search.png" alt="">
        </div>
        <div class="open-search" v-if="isShow">
            <div class="search">
                <img src=".././assets/search-light.png" alt="">
            </div>
            <input type="text" class="open-search-input" placeholder="Поиск">
            <div class="search search-non-border">
                <img src=".././assets/filter.png" v-on:click="showFullFilterModal" alt="">
            </div>
            <div class="search">
                <img src=".././assets/open.png" alt="" v-bind:class="{ active: isFullOpen }" v-on:click="toggleFullOpen">
            </div>
        </div>
        <FullOpenSearcher :filters="selectedFilter" ></FullOpenSearcher>
        <modal name="full-filter" :height="'auto'">
            <FullFilterBlank v-on:set-filter="setFilter"></FullFilterBlank>
        </modal>
    </div>
  </template>
  
  <script>
  import FullOpenSearcher from "./FullOpenSearcher.vue"
  import FullFilterBlank from "./FullFilterBlank.vue"
  export default {
    name: 'SearcherComponent',
    components: {
        FullOpenSearcher,
        FullFilterBlank
    },
    data(){
        return {
            isShow: false,
            isFullOpen: false,
            selectedFilter: []
        }
    },
    methods: {
        showFullSearch(){
            this.isShow = !this.isShow;
        },
        toggleFullOpen(){
            this.isFullOpen = !this.isFullOpen;
        },
        showFullFilterModal () {
            this.$modal.show('full-filter');
        },
        hideFullFilterModal () {
            this.$modal.hide('full-filter');
        },
        setFilter(label, value){
            this.selectedFilter.push({label, value});
        }
    },
    props: {
      
    }
    
  }
  </script>

  <style scoped>
    div.search{
        width: 44px;
        height: 40px;
        background: #fff;
        display: flex;
        align-items: center;
        justify-content: center;
        border-radius: 5px;
        cursor: pointer;
    }
    img.active{
        background-color: #FFE6C0;
        padding: 4px;
        border-radius: 5px;
    }
    div.open-search{
        display: flex;
        flex-direction: row;
        flex-wrap: nowrap;
    }
    div.search-non-border{
        border-radius: 0;
    }
  
    .open-search .search:first-child{
        border-radius: 0;
        border-top-left-radius:5px;
        border-bottom-left-radius: 5px;
    }
  
    .open-search .search:last-child{
        border-radius: 0;
        border-top-right-radius:5px;
        border-bottom-right-radius: 5px;
    }
    .open-search-input{
        border: none;
        outline: none;
        font-family: Montserrat;
        font-size: 14px;
        font-weight: 400;
        line-height: 17px;
        letter-spacing: 0em;
        text-align: left;
        color: #212529;
    }
  </style>
  