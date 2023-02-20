<template>
    <div class="full-filter-wrapper">
        <div class="control">
            <img src="./../assets/cross.png" alt="" class="close-modal">
        </div>
        <div class="filter-content">
            <div class="filter-header">
                <h2 class="filter-head">Фильтры</h2>
            </div>
            <div class="filter-maps">
                <div class="filter-header">
                    <img src="./../assets/collapseArrow.png" alt="" class="collapsed" v-on:click="toggleMap" v-if="isCollapsedMap">
                    <img src="./../assets/collapseArrow.png" alt="" class="open" v-on:click="toggleMap" v-else>
                    <p class="filter-text">По месторасположению</p>
                </div>
                <div class="map" v-if="!isCollapsedMap">
                    <span class="minor-text">
                        Укажите на карте зону расположения заявок
                    </span>
                    <div class="syntetic-map"></div>
                </div>
            </div>
            <div class="filter-blank">
                <div class="filter-header">
                    <p class="filter-text">По бланку</p>
                </div>

                <div class="selector-filter">
                    <label for="">Выберите бланк</label>
                    <select id="custom-select" v-model="selectedBlank" v-on:change="$emit('set-filter', 'Бланк', selectedBlank)">
                        <option v-for="blank in blanks" :key="blank.id">{{ blank.name}}</option>
                    </select>
                </div>
                
                <div class="blank-options" v-if="selectedBlank">
                    
                    <template v-for="field in selectedBlankFields">
                        <FilterSelector :key="field.id" v-if="field.type_id == 1" :options="field.params" :label="field.label" selectorText="Введите название или выберите из списка"></FilterSelector>
                        <FilterInput :key="field.id" v-if="field.type_id == 3" :label="field.label" selectorText="Введите число"></FilterInput>
                        <FilterInputDate :key="field.id" v-if="field.type_id == 7" :label="field.label" ></FilterInputDate>
                        <FilterInput :key="field.id" v-if="field.type_id == 2" :label="field.label" selectorText="Введите значение"></FilterInput>
                        <FilterGroup  :key="field.id" v-if="field.type_id == 5" :label="field.label" :selectedBlankId="field.selected_blank" :blanks="blanks" :level="1"></FilterGroup>
                    </template>
                   
                </div>
            </div>
            <div class="actions-filter">
                <button class="filled-action">Применить</button>
                <button class="outline-action">Сбросить</button>
            </div>
        </div>
    </div>
</template>
  
  <script>
  /*
  
  {"items":[
    {"id":960,"name":"\u041e\u0431\u044b\u0447\u043d\u044b\u0439","fields":
    [{"id":13753,"type_id":2,"type":"\u0421\u0442\u0440\u043e\u043a\u0430","required":0,"sku":13753,"skuDisplay":0,"suffix":"stroka","suffixDisplay":0,"prices_label":[],"params":[],"label":"\u0421\u0442\u0440\u043e\u043a\u0430","list_position":0,"selected_price_display":0,"selected_price":null,"selected_blank":null,"selected_user":null,"stage":1},
    {"id":13754,"type_id":3,"type":"\u0427\u0438\u0441\u043b\u043e","required":0,"sku":13754,"skuDisplay":0,"suffix":"chislo","suffixDisplay":0,"prices_label":[],"params":[],"label":"\u0427\u0438\u0441\u043b\u043e","list_position":1,"selected_price_display":0,"selected_price":null,"selected_blank":null,"selected_user":null,"stage":1},
    {"id":13755,"type_id":4,"type":"\u0424\u043e\u0442\u043e","required":0,"sku":13755,"skuDisplay":0,"suffix":"foto","suffixDisplay":0,"prices_label":[],"params":[],"label":"\u0424\u043e\u0442\u043e","list_position":2,"selected_price_display":0,"selected_price":null,"selected_blank":null,"selected_user":null,"stage":1},
    {"id":13756,"type_id":1,"type":"\u0421\u043f\u0438\u0441\u043e\u043a","required":0,"sku":13756,"skuDisplay":0,"suffix":"vypadayushchiy_spisok","suffixDisplay":0,"prices_label":[],"params":[{"id":23750,"label":"\u0417\u043d\u0430\u0447\u0435\u043d\u0438\u04351","sku":null,"suffix":null,"prices":[],"list_position":0},
    {"id":23751,"label":"\u0417\u043d\u0430\u0447\u0435\u043d\u0438\u04352","sku":null,"suffix":null,"prices":[],"list_position":1}],"label":"\u0412\u044b\u043f\u0430\u0434\u0430\u044e\u0449\u0438\u0439 \u0441\u043f\u0438\u0441\u043e\u043a","list_position":3,"selected_price_display":0,"selected_price":null,"selected_blank":null,"selected_user":null,"stage":1}],
    "bgColor":"#ffffff","color":"#000000","mobile":1,"moderate":0},{"id":961,"name":"wqe","fields":[
        {"id":13859,"type_id":2,"type":"\u0421\u0442\u0440\u043e\u043a\u0430","required":0,"sku":13859,"skuDisplay":0,"suffix":"1","suffixDisplay":0,"prices_label":[],"params":[],"label":"1","list_position":0,"selected_price_display":0,"selected_price":null,"selected_blank":null,"selected_user":null,"stage":1},
        {"id":13860,"type_id":1,"type":"\u0421\u043f\u0438\u0441\u043e\u043a","required":0,"sku":13860,"skuDisplay":0,"suffix":"2","suffixDisplay":1,"prices_label":[{"id":1752,"name":"\u0422\u0438\u043f1"}],
        "params":[{"id":23771,"label":"21","sku":null,"suffix":"\u0448\u0442","prices":[{"items_id":23771,"prices_id":1752,"price":"100.00"}],"list_position":0},{"id":23772,"label":"22","sku":null,"suffix":"\u0448\u0442","prices":[{"items_id":23772,"prices_id":1752,"price":"1000.00"}],"list_position":1}],"label":"2","list_position":1,"selected_price_display":0,"selected_price":null,"selected_blank":null,"selected_user":null,"stage":1}],"bgColor":"#ffffff","color":"#000000","mobile":1,"moderate":1},{"id":962,"name":"\u0413\u0440\u0443\u043f\u043f\u044b","fields":[{"id":13861,"type_id":2,"type":"\u0421\u0442\u0440\u043e\u043a\u0430","required":0,"sku":13861,"skuDisplay":0,"suffix":"stroka","suffixDisplay":0,"prices_label":[],"params":[],"label":"\u0421\u0442\u0440\u043e\u043a\u0430","list_position":0,"selected_price_display":0,"selected_price":null,"selected_blank":null,"selected_user":null,"stage":1},{"id":13862,"type_id":3,"type":"\u0427\u0438\u0441\u043b\u043e","required":0,"sku":13862,"skuDisplay":0,"suffix":"chislo","suffixDisplay":0,"prices_label":[],"params":[],"label":"\u0427\u0438\u0441\u043b\u043e","list_position":1,"selected_price_display":0,"selected_price":null,"selected_blank":null,"selected_user":null,"stage":1},{"id":13863,"type_id":5,"type":"\u0411\u043b\u0430\u043d\u043a","required":0,"sku":13863,"skuDisplay":0,"suffix":"ssylka_na_drugoy_blank","suffixDisplay":0,"prices_label":[],"params":[],"label":"\u0421\u0441\u044b\u043b\u043a\u0430 \u043d\u0430 \u0434\u0440\u0443\u0433\u043e\u0439 \u0431\u043b\u0430\u043d\u043a","list_position":2,"selected_price_display":0,"selected_price":null,"selected_blank":960,"selected_user":null,"stage":1}],"bgColor":"#ffffff","color":"#000000","mobile":1,"moderate":0},{"id":963,"name":"\u041f\u043e\u0432\u0442\u043e\u0440\u044f\u044e\u0449\u0438\u0435\u0441\u044f \u0431\u043b\u043e\u043a\u0438","fields":[{"id":13864,"type_id":3,"type":"\u0427\u0438\u0441\u043b\u043e","required":0,"sku":13864,"skuDisplay":0,"suffix":"chislo","suffixDisplay":0,"prices_label":[],"params":[],"label":"\u0427\u0438\u0441\u043b\u043e","list_position":0,"selected_price_display":0,"selected_price":null,"selected_blank":null,"selected_user":null,"stage":1},{"id":13865,"type_id":2,"type":"\u0421\u0442\u0440\u043e\u043a\u0430","required":0,"sku":13865,"skuDisplay":0,"suffix":"stroka","suffixDisplay":0,"prices_label":[],"params":[],"label":"\u0421\u0442\u0440\u043e\u043a\u0430","list_position":1,"selected_price_display":0,"selected_price":null,"selected_blank":null,"selected_user":null,"stage":1},{"id":13866,"type_id":8,"type":"\u041f\u043e\u0432\u0442\u043e\u0440\u044f\u044e\u0449\u0438\u0439\u0441\u044f \u0431\u043b\u043e\u043a","required":0,"sku":13866,"skuDisplay":0,"suffix":"povt_blok","suffixDisplay":0,"prices_label":[],"params":[],"label":"\u041f\u043e\u0432\u0442 \u0431\u043b\u043e\u043a","list_position":2,"selected_price_display":0,"selected_price":null,"selected_blank":960,"selected_user":null,"stage":1}],"bgColor":"#ffffff","color":"#000000","mobile":1,"moderate":0},{"id":964,"name":"\u042d\u0442\u0430\u043f\u044b","fields":[{"id":13870,"type_id":2,"type":"\u0421\u0442\u0440\u043e\u043a\u0430","required":0,"sku":13870,"skuDisplay":0,"suffix":"stroka","suffixDisplay":0,"prices_label":[],"params":[],"label":"\u0421\u0442\u0440\u043e\u043a\u0430","list_position":0,"selected_price_display":0,"selected_price":null,"selected_blank":null,"selected_user":null,"stage":1},{"id":13871,"type_id":3,"type":"\u0427\u0438\u0441\u043b\u043e","required":0,"sku":13871,"skuDisplay":0,"suffix":"chislo","suffixDisplay":0,"prices_label":[],"params":[],"label":"\u0427\u0438\u0441\u043b\u043e","list_position":1,"selected_price_display":0,"selected_price":null,"selected_blank":null,"selected_user":null,"stage":1},{"id":13872,"type_id":9,"type":"\u0421\u043c\u0435\u043d\u0438\u0442\u044c \u0438\u0441\u043f\u043e\u043b\u043d\u0438\u0442\u0435\u043b\u044f","required":0,"sku":13872,"skuDisplay":0,"suffix":"etap_2","suffixDisplay":0,"prices_label":[],"params":[],"label":"\u042d\u0442\u0430\u043f 2","list_position":2,"selected_price_display":0,"selected_price":null,"selected_blank":null,"selected_user":937,"stage":2},{"id":13873,"type_id":3,"type":"\u0427\u0438\u0441\u043b\u043e","required":0,"sku":13873,"skuDisplay":0,"suffix":"chislo2","suffixDisplay":0,"prices_label":[],"params":[],"label":"\u0447\u0438\u0441\u043b\u043e2","list_position":3,"selected_price_display":0,"selected_price":null,"selected_blank":null,"selected_user":null,"stage":2},{"id":13874,"type_id":2,"type":"\u0421\u0442\u0440\u043e\u043a\u0430","required":0,"sku":13874,"skuDisplay":0,"suffix":"stroka2","suffixDisplay":0,"prices_label":[],"params":[],"label":"\u0441\u0442\u0440\u043e\u043a\u04302","list_position":4,"selected_price_display":0,"selected_price":null,"selected_blank":null,"selected_user":null,"stage":2}],"bgColor":"#ffffff","color":"#000000","mobile":1,"moderate":0}],"prices":[{"id":1752,"prefix":"tip1","name":"\u0422\u0438\u043f1"}]}
  
  
  
  5 -group
  {"id":12839,"type_id":5,"type":"\u0411\u043b\u0430\u043d\u043a","required":0,"sku":12839,"skuDisplay":0,"suffix":"kastom_gruppa","suffixDisplay":0,"prices_label":[],"params":[],"label":"\u041a\u0430\u0441\u0442\u043e\u043c \u0433\u0440\u0443\u043f\u043f\u0430","list_position":5,"selected_price_display":0,"selected_price":null,"selected_blank":962,"selected_user":null,"stage":1}],"bgColor":"#ffffff","color":"#000000","mobile":0,"moderate":0},
  {"id":962,"name":"123","fields":[
    {"id":12832,"type_id":3,"type":"\u0427\u0438\u0441\u043b\u043e","required":0,"sku":12832,"skuDisplay":0,"suffix":"temperatura","suffixDisplay":0,"prices_label":[],"params":[],"label":"\u0422\u0435\u043c\u043f\u0435\u0440\u0430\u0442\u0443\u0440\u0430","list_position":0,"selected_price_display":0,"selected_price":null,"selected_blank":null,"selected_user":null,"stage":1},
    {"id":12833,"type_id":2,"type":"\u0421\u0442\u0440\u043e\u043a\u0430","required":0,"sku":12833,"skuDisplay":0,"suffix":"temp","suffixDisplay":0,"prices_label":[],"params":[],"label":"\u0422\u0435\u043c\u043f","list_position":1,"selected_price_display":0,"selected_price":null,"selected_blank":null,"selected_user":null,"stage":1}],
    "bgColor":"#ffffff","color":"#000000","mobile":0,"moderate":0}],"prices":[]}

        {"items":[
            {"id":961,"name":"qwe","fields":[
                {"id":12825,
                    "type_id":2,
                    "type":"\u0421\u0442\u0440\u043e\u043a\u0430","required":0,"sku":12825,"skuDisplay":0,"suffix":"name","suffixDisplay":0,"prices_label":[],"params":[],"label":"name","list_position":0,"selected_price_display":0,"selected_price":null,"selected_blank":null,"selected_user":null,"stage":1},
                {"id":12826,"type_id":3,"type":"\u0427\u0438\u0441\u043b\u043e","required":0,"sku":12826,"skuDisplay":0,"suffix":"name2","suffixDisplay":0,"prices_label":[],"params":[],"label":"name2","list_position":1,"selected_price_display":0,"selected_price":null,"selected_blank":null,"selected_user":null,"stage":1}],
                "bgColor":"#ffffff","color":"#000000","mobile":0,"moderate":0}],"prices":[]}
  
        */
  import FilterSelector from "./FilterSelector.vue"
  import FilterInput from "./FilterInput.vue"
  import FilterInputDate from "./FilterInputDate.vue"
  import FilterGroup from "./FilterGroup.vue"
  import axios from "axios"
  export default {
    name: 'FullFilterBlank',
    components: {
        FilterSelector,
        FilterInput,
        FilterInputDate,
        FilterGroup
    },
    data(){
        return {
            isCollapsedMap: true,
            selectedBlank: "",
            blanks: [{"id":961,"name":"Бланк 1","bgColor":"#ffffff","color":"#000000", "fields":[
                {"id":12827,"type_id":1,"type":"\u0421\u043f\u0438\u0441\u043e\u043a","required":0,"sku":12827,"skuDisplay":0,"suffix":"soglasie","suffixDisplay":0,"prices_label":[],"params":[{"id":23750,"label":"\u0414\u0430","sku":null,"suffix":null,"prices":[],"list_position":0},{"id":23751,"label":"\u041d\u0435\u0442","sku":null,"suffix":null,"prices":[],"list_position":1}],"label":"\u0421\u043e\u0433\u043b\u0430\u0441\u0438\u0435","list_position":0,"selected_price_display":0,"selected_price":null,"selected_blank":null,"selected_user":null,"stage":1},
                {"id":12828,"type_id":3,"type":"\u0427\u0438\u0441\u043b\u043e","required":0,"sku":12828,"skuDisplay":0,"suffix":"vozrast","suffixDisplay":0,"prices_label":[],"params":[],"label":"\u0412\u043e\u0437\u0440\u0430\u0441\u0442","list_position":1,"selected_price_display":0,"selected_price":null,"selected_blank":null,"selected_user":null,"stage":1},
                {"id":12829,"type_id":4,"type":"\u0424\u043e\u0442\u043e","required":0,"sku":12829,"skuDisplay":0,"suffix":"lichnoe_foto","suffixDisplay":0,"prices_label":[],"params":[],"label":"\u041b\u0438\u0447\u043d\u043e\u0435 \u0444\u043e\u0442\u043e","list_position":2,"selected_price_display":0,"selected_price":null,"selected_blank":null,"selected_user":null,"stage":1},
                {"id":12830,"type_id":7,"type":"\u0421\u0442\u0430\u0442\u0443\u0441","required":0,"sku":12830,"skuDisplay":0,"suffix":"data","suffixDisplay":0,"prices_label":[],"params":[],"label":"\u0414\u0430\u0442\u0430","list_position":3,"selected_price_display":0,"selected_price":null,"selected_blank":null,"selected_user":null,"stage":1},
                {"id":12831,"type_id":2,"type":"\u0421\u0442\u0440\u043e\u043a\u0430","required":0,"sku":12831,"skuDisplay":0,"suffix":"nazvanie","suffixDisplay":0,"prices_label":[],"params":[],"label":"\u041d\u0430\u0437\u0432\u0430\u043d\u0438\u0435","list_position":4,"selected_price_display":0,"selected_price":null,"selected_blank":null,"selected_user":null,"stage":1},
                {"id":12839,"type_id":5,"type":"\u0411\u043b\u0430\u043d\u043a","required":0,"sku":12839,"skuDisplay":0,"suffix":"kastom_gruppa","suffixDisplay":0,"prices_label":[],"params":[],"label":"\u041a\u0430\u0441\u0442\u043e\u043c \u0433\u0440\u0443\u043f\u043f\u0430","list_position":5,"selected_price_display":0,"selected_price":null,"selected_blank":962,"selected_user":null,"stage":1}]
            },{"id":962,"name":"123","fields":[
    {"id":12832,"type_id":3,"type":"\u0427\u0438\u0441\u043b\u043e","required":0,"sku":12832,"skuDisplay":0,"suffix":"temperatura","suffixDisplay":0,"prices_label":[],"params":[],"label":"\u0422\u0435\u043c\u043f\u0435\u0440\u0430\u0442\u0443\u0440\u0430","list_position":0,"selected_price_display":0,"selected_price":null,"selected_blank":null,"selected_user":null,"stage":1},
    {"id":12833,"type_id":2,"type":"\u0421\u0442\u0440\u043e\u043a\u0430","required":0,"sku":12833,"skuDisplay":0,"suffix":"temp","suffixDisplay":0,"prices_label":[],"params":[],"label":"\u0422\u0435\u043c\u043f","list_position":1,"selected_price_display":0,"selected_price":null,"selected_blank":null,"selected_user":null,"stage":1}],
    "bgColor":"#ffffff","color":"#000000","mobile":0,"moderate":0}]
        }
    },
    methods: {
        toggleMap(){
            this.isCollapsedMap = !this.isCollapsedMap;
        },
        async fetchData() {
            console.log("fetch");
            await axios
                .get("http://localhost:11000/orderForms", {
                    params: {
                        fields: ["id", "name", "bgColor", "color", "fields"]
                    }
                })
                .then(res => {
                    this.blanks = res.data.items;   
                    if(this.blanks.length){
                        this.selectedBlank = this.blanks[0].name;
                    }
                })
        },
    },
    computed:{
        selectedBlankFields(){
            const targetBlank = this.blanks.find((blank)=>{
                return blank.name == this.selectedBlank;
            })
            if(targetBlank){
                return targetBlank.fields;
            }
            else{
                return [];
            }
        },
        
    },
    mounted(){
        console.log("mount");
        this.fetchData();
    },

    props: {
      
    }
    
  }
  </script>
<style>
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
<style scoped>
    
    .filled-action{
        border: none;
        background: #FFD79A;
    }
    .outline-action{
        border: 1px solid #FFD79A;
        background-color: white;
    }
    .actions-filter {
        display:flex;
        flex-direction: row;
        justify-content: flex-end;
        gap: 16px;
    }
    .actions-filter button{
        outline: none;
        border-radius: 5px;
        font-size: 14px;
        line-height: 17px;
        /* identical to box height */
        padding: 8px 16px;
        text-align: center;
        cursor: pointer;
        color: #000000;
    }
    .blank-options{
        display: flex;
        flex-direction: column;
        background: #F6F6F6;
        padding: 12px 16px;
        border-radius: 5px;
        margin-top: 18px;
        gap: 16px;
    }
    div.syntetic-map{
        width: 486px;
        height: 250px;
        border: 1px solid black;
    }

    .minor-text{
        
        font-family: 'Montserrat';
        font-style: normal;
        font-weight: 400;
        font-size: 12px;
        line-height: 15px;
        text-align: left;
        color: #7D8487;
    }
    .filter-header{
        display: flex;
        flex-direction: row;
        gap: 10px;
    }
    .filter-text, .filter-head{
        text-align: left;
    }
    .open{
        transform: rotate(90deg);
    }
    .collapsed, .open{
        text-align: left;
        cursor: pointer;
    }
    .filter-text{
        font-family: 'Montserrat';
        font-style: normal;
        font-weight: 600;
        font-size: 14px;
        line-height: 17px;
        color: #212529;
    }
    .filter-head{
        font-family: 'Montserrat';
        font-style: normal;
        font-weight: 600;
        font-size: 16px;
        line-height: 20px;
        color: #212529;

    }
    .filter-content{
        display: flex;
        flex-direction:column;
        margin: 24px 32px;
        gap: 32px;
    }
    .full-filter-wrapper{
        width: auto;
        display: flex;
        flex-direction: column;
        
    }
    .control{
        position: relative;
    }
    .control img{
        position: absolute;
        right: 14px;
        top: 14px;
    }
    .close-modal{
        cursor: pointer;
        margin-left: auto;
    }
</style>