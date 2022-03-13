<template>
  <div v-if="isLoading" class="ml-3 inline-flex rounded-md shadow">
    <button   

   @click="$emit('vote',value)" :class="hoverBg"
      class="inline-flex items-center  justify-center rounded-md border border-transparent bg-white px-5 py-3 text-base font-medium leading-6 text-indigo-600 transition duration-150 ease-in-out hover:text-indigo-500 focus:outline-none"
      > {{title}}</button
    >
  </div>
</template>

<script >
import { defineComponent } from 'vue'

export default defineComponent({
  props:{
     title:{
       type:String,
       default:''
     },
     value:{
       type:String,
       default:0
     },
     isLoading:{
       type:Boolean,
       default:true
     },
     hoverBg:{
       type:String,
         default:'hover:bg-green-200'
     }
  },
  setup(props,context)
  {
    function Vote(value)
    {
// context.emit
    }

    return {
      Vote
    }
  }

})
</script>

