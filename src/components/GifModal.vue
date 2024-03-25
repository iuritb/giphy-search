<template>
  <q-dialog :model-value="showModal" persistent @update:model-value="closeModal">
    <q-card>
      <q-card-section class="q-pa-md">
        <q-img :src="selectedGif.images.original.url" :alt="selectedGif.title" />
      </q-card-section>
      <q-card-section class="q-pa-md">
        <q-btn label="Fechar" color="primary" @click="closeModal" />
      </q-card-section>
    </q-card>
  </q-dialog>
</template>

<script>
export default {
  props: {
    showModal: {
      type: Boolean,
      default: false
    },
    selectedGif: {
      type: Object,
      default: () => ({})
    }
  },
  methods: {
    closeModal(value) {
      this.$emit('update:showModal', value);
    }
  }
}
</script>

<style scoped>
/* Estilos opcionais do modal podem ser adicionados aqui */
</style>
