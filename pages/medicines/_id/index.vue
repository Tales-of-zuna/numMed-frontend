<template>
  <v-container grid-list-xs>
    <v-row
      ><v-col cols="12"
        ><v-card>
          <v-card-title primary-title class="d-flex justify-space-between">
            {{ product.title }}

            <v-btn @click="addProduct = false" icon
              ><v-icon>mdi-close</v-icon>
            </v-btn>
          </v-card-title>
          <v-card-subtitle>{{
            $moment(product.createdAt).format("YYYY-MM-DD HH:mm")
          }}</v-card-subtitle>

          <v-card-text>
            <v-row
              ><v-col cols="4"
                ><v-text-field
                  v-model="product.title"
                  name="name"
                  label="Нэр"
                  id="id"
                  dense
                  color="teal darken-3"
                  outlined
                  hint="Ж: Электрофорез"
                ></v-text-field></v-col
              ><v-col cols="4"
                ><v-text-field
                  name="name"
                  v-model="product.serial"
                  label="Серийн дугаар"
                  color="teal darken-3"
                  id="id"
                  dense
                  hint="ABC123"
                  outlined
                ></v-text-field></v-col
              ><v-col cols="4"
                ><v-text-field
                  name="name"
                  v-model="product.category"
                  label="Ангилал"
                  color="teal darken-3"
                  id="id"
                  dense
                  hint="Ж: Толгойны эм"
                  outlined
                ></v-text-field></v-col></v-row
            ><v-row class="mt-n6"
              ><v-col cols="3"
                ><v-text-field
                  v-model="product.price"
                  name="name"
                  label="Үнэ"
                  color="teal darken-3"
                  id="id"
                  dense
                  outlined
                ></v-text-field></v-col
              ><v-col cols="3"
                ><v-text-field
                  v-model="product.quantity"
                  name="name"
                  label="Тоо ширхэг"
                  color="teal darken-3"
                  id="id"
                  dense
                  outlined
                ></v-text-field></v-col
              ><v-col cols="3"
                ><v-text-field
                  v-model="product.brand"
                  name="name"
                  label="Бренд"
                  color="teal darken-3"
                  id="id"
                  dense
                  outlined
                ></v-text-field></v-col
              ><v-col cols="3">
                <v-text-field
                  name="name"
                  v-model="product.images[0]"
                  label="Зурагны линк"
                  color="teal darken-3"
                  id="id"
                  dense
                  outlined
                ></v-text-field>
              </v-col>
            </v-row>

            <v-row
              ><v-col cols="12"
                ><v-card elevation="0"
                  ><p>
                    Дэлгэрэнгүй мэдээлэл
                    <span class="text--secondary"
                      >( оруулахгүй байж болно )</span
                    >
                  </p>
                  <VueEditor v-model="product.description" /> </v-card></v-col
            ></v-row>
          </v-card-text>

          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn to="/medicines" @click="deleteOne()" color="error"
              >Устгах</v-btn
            >

            <v-btn to="/medicines" color="teal" dark @click="update()">
              Хадгалах
            </v-btn>
          </v-card-actions>
        </v-card></v-col
      ></v-row
    >
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      product: {},
    };
  },
  methods: {
    async update() {
      let response = await this.$axios.put(
        "/product/" + this.$route.params.id,
        this.product
      );
      if (response.status == 200) {
        this.product = response.data;
      }
    },
    async deleteOne() {
      let response = await this.$axios.delete(
        "/product/" + this.$route.params.id
      );
      if (response.status == 200) {
        this.product = response.data;
      }
    },
  },
  async fetch() {
    let response = await this.$axios.get("/product/" + this.$route.params.id);
    if (response.status == 200) {
      this.product = response.data;
    }
  },
  methods: {},
};
</script>

<style></style>
