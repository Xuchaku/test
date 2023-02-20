<template>
    <div class="selector-filter">
        <label for="">{{ label }}</label>
        <div class="selector-group">
            <div class="selector-filter-date">
                <span class="header">С</span>
                <div class="selector-filter-input">
                    <input type="text" class="input-text" placeholder="ДД.ММ.ГГГГ.">
                    <input type="text" class="input-text input-text-mini" placeholder="--:--">
                </div>
            </div>
            <div class="selector-filter-date">
                <span class="header">По</span>
                <div class="selector-filter-input">
                    <input type="text" class="input-text" placeholder="ДД.ММ.ГГГГ.">
                    <input type="text" class="input-text input-text-mini" placeholder="--:--">
                </div>
            </div>
        </div>
    </div>
</template>
  
  <script>
  export default {
    name: 'FilterInputDate',
    props: {
        label: String,
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
  .input-text::placeholder{
        color: #CBCBCB;
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
    .selector-group{
        display: flex;
        flex-direction: row;
        gap: 16px;
    }
    .header{
        font-family: 'Montserrat';
        font-style: normal;
        font-weight: 400;
        font-size: 12px;
        line-height: 15px;
        /* identical to box height */


        /* Grayscale/Grayscale - 800 */

        color: #7D8487; 
    }
    .input-text{
        
    }
    .input-text-mini{
        width: 60px;
    }
    .selector-filter-input{
        display: flex;
        flex-direction: row;
        justify-content: flex-start;
        gap: 8px;
        align-items: flex-start;
    }
  </style>