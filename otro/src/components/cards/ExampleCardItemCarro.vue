<template>
  <div>
<h5>{{product.name}}</h5>
<h5>{{contador}}</h5>
  </div>
</template>

<script>
export default {
  name: "ExampleCardItemCarro",
  data(){
      return{
          contador:0
      }
  },
  props: {
    /**
     * Objeto establecimiento enviado
     * como prop desde una vista
     */
    product: Object,
  },
};
</script>