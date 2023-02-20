
<template>
    <div class="selector-filter">
        <label for="">{{ label }}</label>
        <div class="custom-select-wrapper">
            <div class="custom-select-main" :class="{ active: isOpen }">
                <span v-if="checkedItems.length == 0">{{ selectorText }}</span>
                <div class="chosen-item" v-for="(checkItem, index) in checkedItems" :key="checkItem.id">
                    <span>{{ checkItem }}</span>
                    <img src="./../assets/close.png" alt="" v-on:click="checkedItems.splice(index, 1)">
                </div>
                <img src="./../assets/down.png" alt="" v-on:click="toggleSelector" v-bind:class="{ opened: isOpen }">
            </div>
            <div class="custom-select-choice" v-if="isOpen">
                    <template v-for="item in options">
                        <input type="checkbox" :id="item.id" :value="item.label" v-model="checkedItems"  />
                        <label :for="item.id">
                            <div class="check-box"><img src="./../assets/check.png" alt="" v-if="checkedItems.includes(item.label)"></div>
                            {{item.label}}
                        </label>
                    </template>
            </div>
        </div>
    </div>
  </template>
  
  <script>

  export default {
    name: 'FilterSelector',
    props: {
        label: String,
        selectorText: String,
        options: Array

    },
    components: {

    },
    data(){
        return {
            checkedItems: [],
            isActive: false,
            isOpen: false
        }
    },
    methods: {
        toggleSelector(){
            this.isOpen = !this.isOpen;
        }
    },

    
  }
  </script>

  <style scoped>
  img.opened{
    transform: rotate(180deg);
  }
    div.active{
        border: none;
    }
    .chosen-item{
        display: flex;
        flex-direction: row;
        background: #FFE6C0;
        border-radius: 5px;
        padding: 2px 8px;
        color: #212529;
        gap: 8px;
    }
    .custom-select-choice{
        background-color: white;
        padding: 9px;
        border-bottom-left-radius: 5px;
        border-bottom-right-radius: 5px;
    }
    .custom-select-choice label{
        display: flex;
        flex-direction: row;
        gap: 8px;
        margin-bottom: 10px;
    }
    .check-box{
        width: 14px;
        display:flex;
        align-items: center;
        justify-content: center;
        height: 14px;
        background: #C4C4C4;
        border-radius: 3px;
    }
    .custom-select-wrapper{
        width: 100%;
    }
    .custom-select-choice input[type="checkbox"]{
        display: none;
    }
    .custom-select-choice{
        display: flex;
        flex-direction: column;
    }
    .custom-select-main{
        display: flex;
        gap: 8px;
        flex-direction: row;
        border-radius: 5px;
        border: 1px solid #ccc;
        background-color: white;
        padding: 6px 12px;
        color: #CBCBCB;
        width: 100%;
    }
    .custom-select-main img{
        margin-left: auto;
        cursor: pointer;
    }
    .input-text{
        width: 100%;
        padding: 6px 12px;
        border-radius: 5px;
        outline: none;
        border: 1px solid #ccc;
    }
    .selector-filter{
        display: flex;
        flex-direction: column;
        align-items: flex-start;
    }
    .selector-filter select{
        width: 100%;
        padding: 6px 12px;
    }
  </style>
  