<template>
  <v-container grid-list-xs>
    <v-row
      ><v-col cols="12"
        ><v-snackbar v-model="success">
          Амжилттай нэмэгдлээ <v-icon>mdi-emoticon-cool</v-icon>
          <template v-slot:action="{ attrs }">
            <v-btn dark text v-bind="attrs" @click="success = false">
              <v-icon>mdi-close</v-icon>
            </v-btn>
          </template></v-snackbar
        ><v-snackbar v-model="errorSnack">
          Амжилттай нэмэгдлээ <v-icon>mdi-emoticon-cool</v-icon>
          <template v-slot:action="{ attrs }">
            <v-btn dark text v-bind="attrs" @click="errorSnack = false">
              <v-icon>mdi-close</v-icon>
            </v-btn>
          </template> </v-snackbar
        ><v-card>
          <v-card-title primary-title class="d-flex justify-space-between">
            Эмийн жагсаалт
            <div class="d-flex">
              <v-dialog v-model="addProduct" width="1000">
                <template v-slot:activator="{ on, attrs }">
                  <v-btn color="green " dark small v-bind="attrs" v-on="on"
                    >Эм нэмэх<v-icon small>mdi-plus</v-icon></v-btn
                  >
                </template>
                <v-card>
                  <v-card-title
                    primary-title
                    class="d-flex justify-space-between"
                  >
                    Төхөөрөмж нэмэх
                    <v-btn @click="addProduct = false" icon
                      ><v-icon>mdi-close</v-icon>
                    </v-btn>
                  </v-card-title>

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
                          <VueEditor
                            v-model="product.description"
                          /> </v-card></v-col
                    ></v-row>
                  </v-card-text>

                  <v-card-actions>
                    <v-spacer></v-spacer>

                    <v-btn color="teal" text @click="submitAdd()">
                      Нэмэх
                    </v-btn>
                  </v-card-actions>
                </v-card>
              </v-dialog>
            </div>
          </v-card-title>
          <v-card-subtitle>Нийт: {{ products.length }}</v-card-subtitle>
          <v-card-text>
            <v-text-field
              v-model="search"
              append-icon="mdi-magnify"
              label="Хайх"
              single-line
              :loading="loading"
              loading-text="Уншиж байна... Түр хүлээнэ үү"
              hide-details
              color="grey darken-3"
            ></v-text-field>
          </v-card-text>
          <v-data-table
            show-select
            :footer-props="{
              'items-per-page-text': 'Нэг нүүрэн дэх мөрийн тоо',
            }"
            :headers="headers"
            :items="products"
            :search="search"
          >
            <template v-slot:[`item._id`]="{ item }">
              <v-btn
                class="text-subtitle-2"
                @click="selectProduct(item._id)"
                text
                small
                >{{ item._id }}<v-icon small>mdi-open-in-new</v-icon></v-btn
              >
            </template>

            <template v-slot:[`item.price`]="{ item }">
              {{ formatPrice(item.price) }}
            </template>
            <template v-slot:[`item.createdAt`]="{ item }">
              {{ $moment(item.createdAt).format("YYYY-MM-DD HH:mm") }}
            </template>

            <template v-slot:no-results>
              <span>Бараа олдсонгүй</span><v-icon>mdi-archive-alert</v-icon>
            </template>
          </v-data-table>
        </v-card></v-col
      ></v-row
    >
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      viewProd: false,
      success: false,
      loading: true,
      errorSnack: false,
      addProduct: false,
      product: { images: [] },
      products: [],
      search: "",
      headers: [
        {
          text: "Эмийн дугаар (id)",
          align: "start",
          value: "_id",
        },
        { text: "Нэр ", value: "title" },
        { text: "Серийн дугаар ", value: "serial" },
        { text: "Үйлдвэрлэгч", value: "brand" },
        { text: "Үлдэгдэл", value: "quantity" },
        { text: "Төрөл (category)", value: "category" },
        { text: "Үнэ", value: "price" },
        { text: "Үүсгэсэн он сар", value: "createdAt" },
      ],
    };
  },
  methods: {
    selectProduct(productId) {
      this.$router.push({ name: "medicines-id", params: { id: productId } });
    },
    formatPrice(value) {
      let val = (value / 1).toFixed(0).replace(".");
      return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",") + "₮";
    },
    async submitAdd() {
      if (this.product != { images: [] }) {
        let response = await this.$axios.post("/product/", this.product);
        if (response.status == 200) {
          this.success = true;
          this.addProduct = false;
          this.product = { images: [] };
        } else {
          console.log("boldgue boro");
        }
      } else {
        this.errorSnack = true;
      }
    },
  },
  async fetch() {
    let response = await this.$axios.get("/product/");
    if (response.status == 200) {
      this.products = response.data;
      this.loading = false;
    }
  },
};
</script>

<style></style>
