<template>
   <div class="pokemon">
      <img class="pokemon__img" :src="img" alt="">
      <strong class="pokemon__name">{{name}}<p/>#{{id}}</strong>
   </div>

</template>

<script>
export default {
   name: 'Pokemon',
   props: {
      id: Number,
      name: String,
      img: String
   },

}
</script>


<style lang="scss">

.pokemon {
   justify-content: center;
   background-color: white;
   border-radius: 2px;
   padding: 10px;
   height: 100px;
   width: 170px;

   &__img{
       display: block;
       margin-left: auto;
       margin-right: auto;

   }

   &__name{
      display: flex;
      justify-content: space-between;
      font-family: 'Segoe UI light';
      font-size: 11px;
      font-style: normal;
      text-transform: capitalize;
      margin-top: -7px;
      
   }

   &__id{
       display: right;

       
   }

}
</style>


