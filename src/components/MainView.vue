<template>
  <v-container class="fill-height">
    <v-responsive class="align-centerfill-height mx-auto" max-width="900">
      <v-container>
        <h1>Money flow</h1>
      </v-container>
      <v-container>
        <v-number-input v-model="income" :reverse="false" controlVariant="split" label="Доход" :hideInput="false"
          :inset="false" :min="0" variant="outlined" />
        <v-row>
          <v-col>
            <v-card title="Расходы" :text="`${expenses} BYN`" variant="tonal"></v-card>
          </v-col>
          <v-col>
            <v-card title="Остаток" :text="`${total} BYN`" variant="tonal"></v-card>
          </v-col>
        </v-row>
      </v-container>
      <v-container v-if="sliders.length > 0">
        <div v-for="slider in sliders">
          <CustomSlider class="mb-4" :id="slider.id" :title="slider.title" :min="0" :max="income"
            :limited="slider.limited" @value="getValue" @remove="remove" />
        </div>
      </v-container>
      <v-container v-else>
        <h3 class="text-center">Чтобы добавть новый расход нажмите на кнопку внизу</h3>
      </v-container>
      <v-container>
        <AddModal @save="add" />
      </v-container>
    </v-responsive>
  </v-container>
</template>

<script>
import AddModal from './AddModal.vue';

export default {
  data: () => ({
    income: 0,
    lastId: 2,
    sliders: [
      { id: 1, title: "Магазин", limited: false },
      { id: 2, title: "Собака", limited: false },
    ],
    values: {}
  }),
  computed: {
    expenses() {
      return this.sum()
    },
    total() {
      return this.income - this.sum()
    }
  },
  methods: {
    getValue(slider_value) {
      this.values[slider_value.id] = slider_value.value
    },
    sum() {
      return Object.values(this.values).reduce((partialSum, a) => partialSum + a, 0);
    },
    remove(id) {
      let index = this.sliders.map(x => {
        return x.Id;
      }).indexOf(id);

      this.values[id] = 0;
      this.sliders.splice(index, 1);
    },
    add(name) {
      this.sliders.push({ id: ++this.lastId, title: name, limited: false })
    }
  }
}
</script>
