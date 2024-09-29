<template>
  <v-dialog v-model="isActive" max-width="600">
    <v-card title="Добавить расход">
      <v-card-text>
        <div class="mb-4">
          Введите название для новой категории расходов.
        </div>
        <v-text-field v-model="name" label="Название" variant="outlined"></v-text-field>
      </v-card-text>
      <v-card-actions>
        <v-btn text="Закрыть" @click="close"></v-btn>
        <v-btn text="Добавить" @click="save"></v-btn>
      </v-card-actions>
    </v-card>

    <template v-slot:activator="{ props: activatorProps }">
      <v-btn v-bind="activatorProps" rounded="lg" size="x-large" icon="mdi-plus" block></v-btn>
    </template>

  </v-dialog>
</template>

<script>
export default {
  data: () => (
    {
      name: undefined,
      isActive: false
    }
  ),
  methods: {
    close() {
      this.name = undefined;
      this.isActive = false
    },
    save() {
      if (this.name === undefined || this.name === "") {
        alert("Название не должно быть пустым")
        return;
      }

      this.$emit("save", this.name);
      this.name = undefined;
      this.isActive = false
    }
  }
}
</script>