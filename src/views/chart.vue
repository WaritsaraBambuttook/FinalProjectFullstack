<template>

<div v-if="datasets[0].data.length >0">
    
    <chartjs-line v-bind:labels="labels" v-bind:datasets="datasets" v-bind:option="option"></chartjs-line>
    
    </div>
 
</template>

<script>    
import axios from 'axios'
export default {
    data(){
        return{
            
            labels: [ ],
            datasets:[{
                data:[],
                backgroundColor:["blue"]
            }],
            option:{
                title:{
                    display:true,
                    position:"bottom",
                    text: "Order Chart"
                }
            }
        };
    },
  mounted(){
      var instance = this
      axios
      .get('https://nameless-reaches-35082.herokuapp.com/api/count')
      .then(function(response){
for(var i=0;i<response.data.data.length;i++){
    //console.log(response.data.data[i].id)
instance.labels.push(response.data.data[i].ship_country)
instance.datasets[0].data.push(response.data.data[i].count)}
      })
  }
};
</script>