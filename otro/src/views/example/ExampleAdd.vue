<template>
  <div>
    <h1 class="text-center">Crear establecimiento</h1>
    <ExampleForm />
    <h1>Breiner Agregó esto</h1>
  </div>
</template>

<script>
import ExampleForm from "@/components/forms/ExampleForm.vue";

export default {
  name: "ExampleAdd",
  components: {
    ExampleForm,
  },
};
</script>
