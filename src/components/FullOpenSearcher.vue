<template>
    <div class="full-open-searcher">
        <div class="full-searcher-list">
            <img src=".././assets/search-light.png" alt="">
            <template v-for="filter in filters">
                <div class="filter-item">
                    <span class="filter-position">{{ filter.label }}:</span>
                    <div class="filter-value">
                        <span>{{ filter.value }}</span>
                        <img src=".././assets/close.png" alt="">
                    </div>
                </div>
            </template>
            <!-- <div class="filter-item">
                <span class="filter-position">Бланк:</span>
                <div class="filter-value">
                    <span>Подключение</span>
                    <img src=".././assets/close.png" alt="">
                </div>
            </div>
            <div class="filter-item">
                <span class="filter-position">Клиент:</span>
                <div class="filter-value">
                    <span>Каренин</span>
                    <img src=".././assets/close.png" alt="">
                </div>
            </div> -->
            <img class="close-filter" src=".././assets/close.png" alt="">
        </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'FullOpenSearcher',
    data(){
        return {
          
        }
    },
    methods: {
       
    },
    props: {
        filters: Array
    }
    
  }
  </script>

  <style scoped>
    div.full-open-searcher{
        padding: 8px 12px;
        background: #FFFFFF;
        border-radius: 5px;
    }
    div.full-searcher-list{
        position: relative;
        display: flex;
        flex-direction: row;
        gap: 12px;
        align-items: center;
        flex-wrap: wrap;
        border-radius: 5px;
        padding: 8px 12px;
        border: 1px solid #ccc;
    }
    img.close-filter{
        position: absolute;
        right: 15px;
        cursor: pointer;
        top:50%;
        transform: translateY(-50%);
    }
    div.filter-item{
        display: flex;
        padding: 4px;
        align-items: center;
        gap: 4px;
        border-radius: 5px;
        background: #FFF4E4;
    }
    div.filter-value{
        background: #FFE6C0;
        border-radius: 5px;
        display: flex;
        gap: 8px;
        padding: 4px;

    
    }
    div.filter-value img{
        cursor: pointer;
    }
    div.full-searcher-list > img{
        width: 20px;
        height: 20px;
    }
  </style>