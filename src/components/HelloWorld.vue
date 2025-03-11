<template>
  <v-layout wrap pt-6 pt-lg-6 pa-2 pa-lg-8>
    <v-flex xs12>
      <v-layout>
        <v-flex xs12 class="heading"> Add new item </v-flex>
      </v-layout>
      <v-layout wrap py-3>
        <v-flex xs12 sm6 lg8 pa-1>
          <v-form ref="menuForm">
            <v-layout wrap class="box-border" pa-7 pa-lg-7>
              <v-flex xs4 pt-2 class="sub-heading" align-self-center>
                Item Name *
              </v-flex>
              <v-flex xs8 pt-2>
                <v-text-field
                  :rules="[(v) => !!v || 'Required']"
                  v-model="new_item.name"
                  label="Item name"
                  placeholder="Dense & Rounded"
                  filled
                  rounded
                  dense
                ></v-text-field>
              </v-flex>
              <v-flex xs4 pt-2 class="sub-heading" align-self-center>
                Description *
              </v-flex>
              <v-flex xs8 pt-2>
                <v-textarea
                  :rules="[(v) => !!v || 'Required']"
                  v-model="new_item.description"
                  label="item description"
                  placeholder="Dense & Rounded"
                  filled
                  rounded
                  dense
                  counter
                  maxlength="150"
                ></v-textarea>
              </v-flex>
              <v-flex xs4 pt-2 class="sub-heading" align-self-center>
                Upload photo
              </v-flex>
              <v-flex xs8 pt-2>
                <input
                  accept="image/png,  image/jpeg"
                  type="file"
                  ref="userimg"
                  @change="changeImage"
                  style="display: none"
                />

                <v-card
                  elevation="0"
                  @click="uploadImage()"
                  height="160px"
                  style="border-radius: 60px"
                  color="#f2f2f2"
                >
                  <v-layout wrap fill-height>
                    <v-flex pt-4 xs12 text-center>
                      <img
                        v-if="imgurl"
                        height="80px"
                        width="80px"
                        :src="imgurl"
                      />
                    </v-flex>
                    <v-flex xs12 align-self-center text-center>
                      Upload Photo <v-icon>mdi-upload</v-icon>
                    </v-flex>
                  </v-layout>
                </v-card>
              </v-flex>

              <v-flex xs4 pt-6 class="sub-heading" align-self-center>
                category *
              </v-flex>
              <v-flex xs8 pt-6>
                <v-select
                  :rules="[(v) => !!v || 'Required']"
                  :items="['Vegetarian', 'Non-vegetarian']"
                  v-model="new_item.category"
                  label="item Category"
                  placeholder="Dense & Rounded"
                  filled
                  rounded
                  dense
                ></v-select>
              </v-flex>

              <v-flex xs4 pt-2 class="sub-heading" align-self-center>
                Dish type *
              </v-flex>
              <v-flex xs8 pt-2>
                <v-select
                  v-model="new_item.type"
                  :rules="[(v) => !!v || 'Required']"
                  :items="[
                    'Appetizers ',
                    'Main Course',
                    'Side Dishes',
                    'Desserts',
                    'Beverages',
                  ]"
                  label="Dish type"
                  placeholder="Dense & Rounded"
                  filled
                  rounded
                  dense
                ></v-select>
              </v-flex>
              <v-flex xs4 pt-2 class="sub-heading" align-self-center>
                Amount *
              </v-flex>
              <v-flex xs4 pt-2 pr-1>
                <v-text-field
                  :rules="[(v) => !!v || 'Required']"
                  type="number"
                  label="Enter Amount"
                  v-model="new_item.price"
                  placeholder="Dense & Rounded"
                  filled
                  rounded
                  dense
                ></v-text-field>
              </v-flex>
              <v-flex xs4 pt-2 pl-1>
                <v-select
                  :rules="[(v) => !!v || 'Required']"
                  :items="[
                    'Pieces',
                    'litre',
                    'kg',
                    'Slice',
                    'Chunk ',
                    'Cup ',
                    'Plate ',
                    'Scoop ',
                  ]"
                  v-model="new_item.quantity"
                  label="Quantity"
                  placeholder="Dense & Rounded"
                  filled
                  rounded
                  dense
                ></v-select>
              </v-flex>
            </v-layout>

            <v-layout wrap class="box-border" pa-7 mt-2 pa-lg-7>
              <v-flex xs12 class="heading">Additional Charges </v-flex>
              <v-flex xs4 pt-2 class="sub-heading" align-self-center>
                Packing charges
              </v-flex>
              <v-flex xs8 pt-2>
                <v-text-field
                type="number"
                  v-model="new_item.packing_charge"
                  label="Packing charge"
                  placeholder="Dense & Rounded"
                  filled
                  rounded
                  dense
                ></v-text-field>
              </v-flex>

              <v-flex xs4 pt-2 class="sub-heading" align-self-center>
                Delivery Charges
              </v-flex>
              <v-flex xs8 pt-2>
                <v-text-field
                type="number"
                  v-model="new_item.delivery_charge"
                  label="Delivery charge"
                  placeholder="Dense & Rounded"
                  filled
                  rounded
                  dense
                ></v-text-field>
              </v-flex>
            </v-layout>

            <v-layout wrap pt-8 justify-center>
              <v-flex xs12 sm8 text-center>
                <v-btn
                  @click="addItem()"
                  outlined
                  dark
                  color="#ff6600"
                  rounded
                  x-large
                  block
                >
                  <span>Add Item</span>
                </v-btn>
              </v-flex>
            </v-layout>
          </v-form>
        </v-flex>
        <v-flex xs12 sm6 lg4 pa-1>
          <v-layout wrap class="box-border" pa-7 pa-lg-7>
            <v-flex xs12 v-if="added_items.length == 0">
              <v-layout wrap>
                <v-flex xs12 text-center>
                  <v-icon size="200"> mdi-note-plus-outline </v-icon>
                </v-flex>
                <v-flex xs12 py-8 class="sub-heading" text-center>
                  Empty List....!
                </v-flex>
              </v-layout>
            </v-flex>
            <v-flex xs12 v-for="(item, i) in added_items" :key="i">
              <v-card outlined elevation="0" class="mt-2">
                <v-layout wrap pa-4 mt-2>
                  <v-flex shrink pr-2>
                    <v-avatar v-if="item.image" size="56">
                      <img height="80px" width="80px" :src="item.image" />
                    </v-avatar>
                    <v-avatar color="#f2f2f2" v-else size="56">
                      <v-icon> mdi-image-off </v-icon>
                    </v-avatar>
                  </v-flex>
                  <v-flex align-self-center xs2 class="sub-heading" text-center>
                    <v-icon :color="getcatColor(item)"
                      >mdi-square-circle</v-icon
                    >

               
                  </v-flex>
                  <v-flex align-self-center class="sub-heading" text-center>
                    {{ item.name }}
                  </v-flex>
                  <v-flex align-self-center text-right>
                    <v-btn icon @click="added_items.splice(i, 1)">
                      <v-icon>mdi-trash-can</v-icon>
                    </v-btn>
                  </v-flex>
                </v-layout>
              </v-card>
            </v-flex>
            <v-flex xs12 text-center pt-5>
              <v-layout wrap justify-center>
                <v-flex xs12 text-center>
                  <v-btn
                    :disabled="added_items.length == 0 ? true : false"
                    color="#ff6600"
                    rounded
                    x-large
                    block
                  >
                    <span>Upload </span>
                  </v-btn>
                </v-flex>
              </v-layout>
            </v-flex>
          </v-layout>
        </v-flex>
      </v-layout>

      <v-snackbar v-model="snackbar" right :timeout="2000">
        {{ snackbar_msg }}

        <template v-slot:action="{ attrs }">
          <v-btn color="pink" text v-bind="attrs" @click="snackbar = false">
            Close
          </v-btn>
        </template>
      </v-snackbar>
    </v-flex>
  </v-layout>
</template>
<script>
export default {
  name: "HelloWorld",

  data: () => ({
    new_item: {},
    added_items: [],
    snackbar: false,
    imgurl: null,

    snackbar_msg: "",
  }),
  watch: {
    "new_item.description"(newVal) {
      if (newVal) {
        this.new_item.description =
          newVal.charAt(0).toUpperCase() + newVal.slice(1).toLowerCase();
      }
    },
    "new_item.name"(newVal) {
      if (newVal) {
        this.new_item.name =
          newVal.charAt(0).toUpperCase() + newVal.slice(1).toLowerCase();
      }
    },
  },
  methods: {
    addItem() {
      if (this.$refs.menuForm.validate()) {
      this.added_items.push(this.new_item);
      this.$refs.menuForm.resetValidation();
      this.snackbar = true;
      this.new_item = {};
      this.imgurl = null;
      this.snackbar_msg = " Item successfully added to the list";
      }
    },

    getcatColor(item) {
      if (item.category == "Vegetarian") return "#009900";
      return "#cc0000";
    },

    uploadImage() {
      this.$refs.userimg.click();
    },

    changeImage(e) {
      let img = e.target.files[0];
      if (!img) return;
      const maxSize = 40 * 1024 * 1024;
      if (img.size > maxSize) {
        this.snackbar = true;
        this.snackbar_msg =
          "File size exceeds 40MB. Please select a smaller file.";

        e.target.value = "";
        return;
      }
      this.imgurl = URL.createObjectURL(img);
      this.new_item.image = this.imgurl;
      e.target.value = "";
    },
  },
};
</script>
