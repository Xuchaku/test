<template>
    <div class="selector-filter">
        <div class="filter-header">
            <img src="./../assets/collapseArrow.png" alt="" class="collapsed" v-on:click="toggleGroup" v-if="isCollapsedGroup">
            <img src="./../assets/collapseArrow.png" alt="" class="open" v-on:click="toggleGroup" v-else>
            <label for="">{{ label }}</label>
        </div>
        <div class="filter-group" v-if="!isCollapsedGroup && selectedBlankId" :style="{ marginLeft: level * 10 + 'px' }">
            <template v-for="field in selectedBlankFields">
                
                <FilterSelector :key="field.id" v-if="field.type_id == 1" :options="field.params" :label="field.label" selectorText="Введите название или выберите из списка"></FilterSelector>
                <FilterInput :key="field.id" v-if="field.type_id == 3" :label="field.label" selectorText="Введите число"></FilterInput>
                <FilterInputDate :key="field.id" v-if="field.type_id == 7" :label="field.label" ></FilterInputDate>
                <FilterInput :key="field.id" v-if="field.type_id == 2" :label="field.label" selectorText="Введите значение"></FilterInput>
                <FilterGroup :key="field.id" v-if="field.type_id == 5" :label="field.label" :selectedBlankId="field.selected_blank" :blanks="blanks" :level="level + 1"></FilterGroup>
            </template>
        </div>
        
    </div>
</template>
  
  <script>
  import FilterSelector from "./FilterSelector.vue"
  import FilterInput from "./FilterInput.vue"
  import FilterInputDate from "./FilterInputDate.vue"
  export default {
    name: 'FilterGroup',
    components: {
        FilterSelector,
        FilterInput,
        FilterInputDate
    },
    props: {
        label: String,
        selectedBlankId: Number,
        blanks: Array,
        level: Number
    },
    data(){
        return {
            isCollapsedGroup: true
        }
    },
    computed:{
        selectedBlankFields(){
            const targetBlank = this.blanks.find((blank)=>{
                return blank.id == this.selectedBlankId;
            })
            if(targetBlank){
                return targetBlank.fields;
            }
            else{
                return [];
            }
        },
        
    },
    methods: {
        toggleGroup(){
            this.isCollapsedGroup = !this.isCollapsedGroup;
        }
        
    },

    
  }
  </script>

  <style scoped>
    .filter-group{
        display: flex;
        flex-direction: column;
        gap: 16px;
        width: 100%;
    }
    .filter-header{
        display: flex;
        flex-direction: row;
        gap: 10px;
        margin-bottom: 10px;
    }
    .collapsed, .open{
        text-align: left;
        cursor: pointer;
    }
    .open{
        transform: rotate(90deg);
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
  </style>