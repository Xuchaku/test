<template>
    <div class="selector-filter">
        <label for="">{{ label }}</label>
        <input type="text" :placeholder="selectorText" class="input-text">
    </div>
</template>
  
  <script>
  export default {
    name: 'FilterInput',
    props: {
        label: String,
        selectorText: String,
    },
    data(){
        return {
          
        }
    },
    methods: {
       
    },

    
  }
  </script>

  <style scoped>
   .input-text{
        width: 100%;
        padding: 6px 12px;
        border-radius: 5px;
        outline: none;
        border: 1px solid #ccc;
    }
    .input-text::placeholder{
        color: #CBCBCB;
    }
    .selector-filter{
        display: flex;
        flex-direction: column;
        align-items: flex-start;
    }
  </style>