<template>
  <div class="container">
    <div class="row">
      <div class="col-6">
        <center>
          <br />

          <h5>LISTADO DE PRODUCTOS</h5>
        </center>
        <br />
        <div
          class="col-xl-4 col-md-6 col-sm-12"
          v-for="product in allProducts.edges"
          :key="product.node.id"
        >
          <ExampleCard :product="product.node" />
          <button class="btn btn-primary" v-on:click="agregarItem(product)">
            Agregar
          </button>
          <br />
          <hr />
        </div>
      </div>

      <div class="col-6">
        <br />
        <h5>CARRITO DE COMPRAS</h5>
        <br />

        <div class="card border-dark mb-3">
          <div class="row">
            <div class="col mt-3" style="text-align: left; margin-left: 10px">
              Mi pedido
            </div>
            <div class="col"></div>
            <div class="col mt-2" style="text-align: right">
              <button
                type="button"
                class="btn btn-light btn-outline btn-xs"
                v-on:click="limpiarCarrito()"
              >
                <span>
                  <i class="icon ion-md-trash lead align-middle mr-2"></i>
                </span>
              </button>
            </div>
          </div>
          <br />
          <!--Items-->
          <span v-for="(item, indice) of this.items" v-bind:key="item.id">
            <div class="row ml-3" v-if="indice != 0">
              <div class="col">
                <button
                  class="btn btn-light btn-outline btn-xs"
                  v-on:click="decrementarContador(item.producto.id)"
                >
                  -
                </button>
                {{ items[indice].counter }}

                <button
                  class="btn btn-light btn-outline btn-xs"
                  v-on:click="incrementarContador(item.producto.id)"
                >
                  +
                </button>
              </div>

              <div class="col text-left">
                {{ items[indice].producto.name }}
              </div>
              <div class="col text-left">
                {{ items[indice].producto.price * items[indice].counter }}
              </div>
              <div class="col" style="text-align: right; margin-right: 2%">
                <button
                  type="button"
                  class="btn btn-xs"
                  v-on:click="eliminarItem(item.producto.id)"
                >
                  x
                </button>
              </div>
            </div>
          </span>
          <!--costos-->
          <br />
          <div class="row ml-3">
            <div class="col">
              <p style="text-align: left; margin-left: 10px">Envio</p>
              <p style="text-align: left; margin-left: 10px">Sub-Total</p>
              <p style="text-align: left; margin-left: 10px; font-weight: bold">
                Total
              </p>
            </div>
            <div class="col">
              <p>$ 0</p>
              <p>$ 0</p>
              <p style="font-weight: bold">$</p>
            </div>
          </div>
          <!--
          <div class="">
            <div class="row" v-if="indice != 0">
              <ul class="">
                <li
                  v-for="(item, indice) of this.items"
                  v-bind:key="item.id"
                  class="list-group-item"
                >
                  <button
                    class="btn btn-light btn-outline btn-xs"
                    v-on:click="decrementarContador(item.producto.id)"
                  >
                    -
                  </button>
                  {{ items[indice].counter }}

                  <button
                    class="btn btn-light btn-outline btn-xs"
                    v-on:click="incrementarContador(item.producto.id)"
                  >
                    +
                  </button>
                  {{ items[indice].producto.name }}

                  {{ items[indice].producto.price }}

                  <span class="badge badge-pill">
                    <button
                      type="button"
                      class="btn btn-xs"
                      v-on:click="eliminarItem(item.producto.id)"
                    >
                      x
                    </button>
                  </span>
                </li>
              </ul>
            </div>

            <a href="#" class="btn btn-success">Continuar</a>
          </div>
          -->
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ExampleCard from "@/components/cards/ExampleCard.vue";

export default {
  name: "ExampleList",
  components: {
    ExampleCard,
  },
  data() {
    return {
      // Variable que recibe los resultados
      // de la consulta definida en la sección apollo
      allProducts: Object,
      // Variable que recibe el error de la consulta
      items: [{ producto: Object, counter: null }],

      error: null,
    };
  },
  methods: {
    agregarItem(product) {
      var bandera = false;
      console.log(product.node.id);
      console.log(this.items.length);
      if (this.items.length == 0) {
        this.items.push({ producto: product.node, counter: 1 });
      } else {
        for (var index = 1; index < this.items.length; index++) {
          console.log("comparacion");
          console.log(product.node.id);
          console.log(this.items[index].producto.id);
          if (product.node.id == this.items[index].producto.id) {
            this.items[index].counter++;
            bandera = true;
          }
        }
        if (bandera == false) {
          this.items.push({ producto: product.node, counter: 1 });
        }
      }
    },

    eliminarItem(id) {
      console.log(id);

      for (var index = 1; index < this.items.length; index++) {
        if (id == this.items[index].producto.id) {
          this.items.splice(index, 1);
        }
      }
    },
    incrementarContador(id) {
      console.log(id);

      for (var index = 1; index < this.items.length; index++) {
        if (id == this.items[index].producto.id) {
          this.items[index].counter++;
        }
      }
    },
    decrementarContador(id) {
      console.log(id);
      for (var index = 1; index < this.items.length; index++) {
        if (
          id == this.items[index].producto.id &&
          this.items[index].counter < 2
        ) {
          this.items.splice(index, 1);
        } else {
          if (
            id == this.items[index].producto.id &&
            this.items[index].counter >= 2
          ) {
            this.items[index].counter--;
          }
        }
      }
    },

    limpiarCarrito() {
      this.items.splice(1, this.items.length);
    },

    existe() {
      if (this.items.nombreItem != "Pizza") {
        this.agregarItem();
      } else {
        this.incrimentarContador();
      }
    },
  },
  apollo: {
    allProducts: {
      // Consulta
      query: require("@/graphql/enterprise/allProducts.gql"),
      // Asigna el error a la variable definida en data
      error(error) {
        this.error = JSON.stringify(error.message);
      },
    },
  },
};
</script>
