<template>
  <v-card class="mx-auto">
    <v-toolbar dense flat>
      <v-toolbar-title>
        <span class="text-subheading">{{ title }}</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn icon="mdi-delete" variant="text" @click="remove"></v-btn>
    </v-toolbar>
    <v-card-text>
      <v-row class="mb-1" justify="space-between">
        <v-col class="text-left">
          <span class="text-h2 font-weight-light" v-text="value"></span>
          <span class="subheading font-weight-light me-1 ml-3">BYN</span>
        </v-col>
      </v-row>

      <v-slider @update:model-value="update_value" v-model="value" :color="color" :step="1" :max="max" :min="min"
        track-color="grey">
        <template v-slot:prepend>
          <v-btn :color="color" icon="mdi-minus" size="small" variant="text" @click="decrement"></v-btn>
        </template>

        <template v-slot:append>
          <v-btn :color="color" icon="mdi-plus" size="small" variant="text" @click="increment"></v-btn>
        </template>
      </v-slider>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  props: {
    id: Number,
    title: String,
    min: Number,
    max: Number
  },

  data: () => ({
    value: 0,
    isPositiveDirection: null
  }),

  computed: {
    color() {
      if (this.percent() < 0.25) return 'teal'
      if (this.percent() < 0.5) return 'green'
      if (this.percent() < 0.75) return 'orange'
      return 'red'
    }
  },
  methods: {
    update_value(current_value) {
      this.$emit("value", { id: this.$props["id"], value: current_value })
    },
    decrement() {
      if (this.value === this.min)
        return;

      this.value--
      this.$emit("value", { id: this.$props["id"], value: this.value })
    },
    increment() {
      if (this.value === this.max)
        return;

      this.value++
      this.$emit("value", { id: this.$props["id"], value: this.value })
    },
    percent() {
      return this.value / this.max
    },
    remove() {
      this.$emit("remove", this.$props["id"])
    }
  },
  mounted() {
    this.value = this.$props["min"]
  }
}
</script>

<style>
@keyframes metronome-example {
  from {
    transform: scale(.5);
  }

  to {
    transform: scale(1);
  }
}

.v-avatar--metronome {
  animation-name: metronome-example;
  animation-iteration-count: infinite;
  animation-direction: alternate;
}
</style>