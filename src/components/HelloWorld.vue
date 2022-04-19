<template>
  <v-container>
    <v-card class="mx-15">
      <v-card-title class="justify-center">FORM PENJUALAN </v-card-title>
      <v-card-title class="justify-center">
        PT. JAYA ABADI SELAMANYA
      </v-card-title>
      <v-card-text class="mt-5">
        <v-form ref="form" v-model="valid">
          <v-row>
            <v-col>
              <v-text-field
                outlined
                label="Nama Item"
                v-model="newItem.nama"
                :rules="[rules.required]"
              ></v-text-field>
            </v-col>
            <v-col>
              <v-text-field
                outlined
                label="Qty"
                v-model="newItem.qty"
                :rules="[rules.required]"
                @keypress="isNumber($event)"
              ></v-text-field>
            </v-col>
            <v-col>
              <v-text-field
                outlined
                label="Harga"
                v-model="newItem.harga"
                :rules="[rules.required]"
                @keypress="isNumber($event)"
              ></v-text-field>
            </v-col>
            <v-col>
              <v-btn color="error" :disabled="!valid" @click.prevent="addItem">
                <v-icon>mdi-plus</v-icon>
              </v-btn>
            </v-col>
          </v-row>
        </v-form>
      </v-card-text>
      <v-card-text class="mt-5">
        <v-data-table
          class="black--text"
          :headers="headers"
          :items="listItem"
          :loading="loading"
          hide-default-footer
        >
          <template v-slot:[`item.no`]="{ index }">
            {{ index + 1 }}
          </template>

          <template v-slot:[`item.harga`]="{ item }">
            Rp. {{ moneyFormat(item.harga) }}
          </template>

          <template v-slot:[`item.subtotal`]="{ item }">
            Rp. {{ subtotal(item.qty, item.harga) }}
          </template>

          <template v-slot:[`item.aksi`]="{ index }">
            <v-btn
              fab
              dark
              x-small
              color="red"
              class="text-caption mr-3 my-2"
              @click.prevent="deleteItem(index)"
            >
              <v-icon>mdi-close-thick</v-icon>
            </v-btn>
          </template>
        </v-data-table>
      </v-card-text>
    </v-card>
  </v-container>
</template>

<script>
export default {
  name: "HelloWorld",

  data: () => ({
    valid: false,
    loading: false,
    headers: [
      {
        text: "No",
        class: "text-subtitle-1 font-weight-bold black--text",
        value: "no",
      },
      {
        text: "Nama Item",
        class: "text-subtitle-1 font-weight-bold black--text",
        value: "nama",
      },
      {
        text: "Qty",
        class: "text-subtitle-1 font-weight-bold black--text",
        value: "qty",
      },
      {
        text: "Harga",
        class: "text-subtitle-1 font-weight-bold black--text",
        value: "harga",
      },
      {
        text: "Subtotal",
        class: "text-subtitle-1 font-weight-bold black--text",
        value: "subtotal",
      },
      {
        text: "Aksi",
        class: "text-subtitle-1 font-weight-bold black--text",
        value: "aksi",
      },
    ],
    listItem: [],
    newItem: {
      nama: "",
      qty: "",
      harga: "",
    },
    rules: {
      required: (value) => !!value || "Tidak boleh kosong !!",
    },
  }),

  methods: {
    addItem() {
      this.listItem.push(this.newItem);
      this.clearInput();
    },
    deleteItem(idx) {
      console.log(idx);
      this.listItem.splice(idx, 1);
    },
    clearInput() {
      this.newItem = {
        nama: "",
        qty: "",
        harga: "",
      };
      this.$refs.form.resetValidation();
    },

    subtotal(qty, harga) {
      return this.moneyFormat(Number(qty * harga));
    },

    totalQty(val) {
      return val;
    },

    moneyFormat(val) {
      return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
    },

    isNumber(evt) {
      evt = evt ? evt : window.event;
      var charCode = evt.which ? evt.which : evt.keyCode;
      if (
        charCode > 31 &&
        (charCode < 48 || charCode > 57) &&
        charCode !== 46
      ) {
        evt.preventDefault();
      } else {
        return true;
      }
    },
  },
};
</script>