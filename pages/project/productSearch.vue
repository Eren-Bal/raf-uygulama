<template>
  <v-container class="container" style="max-width: 1000px">
    <v-row>
      <v-col cols="6">
        <v-text-field
          label="Sipariş Numarası"
          v-model="project.orderNumber"
          outlined
          dense
          hide-details
        ></v-text-field>
      </v-col>
      <v-col cols="4">
        <v-select
          label="Bulunduğu Raf"
          v-model="project.shelf"
          outlined
          dense
          hide-details
          :items="shelves"
        ></v-select>
      </v-col>
      <v-col cols="2">
        <v-row justify="center">
          <v-switch
            v-model="varMi"
            :label="`Ribana ${varMi ? 'Var' : 'Yok'}`"
          ></v-switch>
        </v-row>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="4">
        <v-text-field
          label="Model Adı"
          v-model="project.modelName"
          outlined
          dense
          hide-details
        ></v-text-field>
      </v-col>
      <v-col cols="4">
        <v-text-field
          label="İsteme Tarihi"
          v-model="project.desiredDate"
          outlined
          dense
          hide-details
        ></v-text-field>
      </v-col>
      <v-col cols="4">
        <v-text-field
          label="Çalışma Numarası"
          v-model="project.workNumber"
          outlined
          dense
          hide-details
        ></v-text-field>
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="4">
        <select-date
          :label="'Ürün Giriş Tarihi'"
          @date="handleRequestDate"
        ></select-date>
      </v-col>
      <v-col cols="4">
        <select-date
          :label="'Ürün Çıkış Tarihi'"
          @date="handleDeliveryDate"
        ></select-date>
      </v-col>
      <v-col cols="2">
        <v-btn color="success" block><v-icon>mdi-content-save</v-icon></v-btn>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
class POShelf {
  constructor(
    deliveryDate,
    requestDate,
    orderNumber,
    shelf,
    modelName,
    desiredDate,
    workNumber
  ) {
    this.deliveryDate = deliveryDate;
    this.requestDate = requestDate;
    this.orderNumber = orderNumber;
    this.shelf = shelf;
    this.modelName = modelName;
    this.desiredDate = desiredDate;
    this.workNumber = workNumber;
  }
}
export default {
  data() {
    return {
      project: new POShelf(),
      varMi: false, // varMi değerini başlangıç değeri olarak false yaptık ve tıkladığımız zaman bu değer true oluyor bize Var diyor
      shelves: [
        { value: "A1", text: "A1" },
        { value: "A2", text: "A2" },
        { value: "B1", text: "B1" },
        { value: "B2", text: "B2" },
      ],
    };
  },

  methods: {
    handleRequestDate(date) {
      this.project.requestDate = date;
    },
    handleDeliveryDate(date) {
      this.project.deliveryDate = date;
    },
  },
};
</script>

<style></style>
