<template>
  <div>
<h5>{{product.name}}</h5>
<h5>{{product.price}}</h5>
  </div>
</template>

<script>
export default {
  name: "ExampleCardItemCarro",
  props: {
    /**
     * Objeto establecimiento enviado
     * como prop desde una vista
     */
    product: Object,
  },
};
</script>
