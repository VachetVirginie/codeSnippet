<template>
  <div class="playground-container">
    <h1>SyTextField Playground</h1>

    <div class="controls">
      <h3>Options</h3>
      <div class="control-row">
        <label>
          <input type="checkbox" v-model="isValidatedOnBlur">
          Valider au blur
        </label>
        <label>
          <input type="checkbox" v-model="isDirty">
          Dirty
        </label>
        <label>
          <input type="checkbox" v-model="isDisabled">
          Disabled
        </label>
        <label>
          <input type="checkbox" v-model="displayAsterisk">
          Afficher astérisque
        </label>
        <label>
          <input type="checkbox" v-model="required">
          Required
        </label>
      </div>

      <div class="control-row">
        <label>
          Variante:
          <select v-model="variant">
            <option value="outlined">Outlined</option>
            <option value="underlined">Underlined</option>
          </select>
        </label>
        <label>
          Icon append:
          <select v-model="appendInnerIcon">
            <option value="">Aucun</option>
            <option value="success">Success</option>
            <option value="error">Error</option>
            <option value="info">Info</option>
          </select>
        </label>
      </div>
    </div>

    <div class="textfield-container">
      <SyTextField
          v-model="textValue"
          :label="'Champ texte'"
          :required="required"
          :rules="validationRules"
          :variant="variant"
          :display-asterisk="displayAsterisk"
          :is-validate-on-blur="isValidatedOnBlur"
          :is-disabled="isDisabled"
          :is-dirty="isDirty"
          :append-inner-icon="info"
          @blur="handleBlur"
          @input="handleInput"
      >
        <template v-if="showAppendSlot" #append-inner>
          <VIcon color="primary">
            mdi-check
          </VIcon>
        </template>
      </SyTextField>
    </div>

    <div class="events-log">
      <h3>Événements</h3>
      <div class="log-container">
        <div v-for="(log, index) in eventLogs" :key="index" class="log-item">
          {{ log }}
        </div>
      </div>
      <button @click="clearLogs">Effacer les logs</button>
    </div>

    <div class="value-display">
      <h3>Valeur actuelle</h3>
      <pre>{{ textValue }}</pre>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import SyTextField from '@/components/Customs/SyTextField/SyTextField.vue';

const textValue = ref('');
const isValidatedOnBlur = ref(true);
const isDirty = ref(false);
const isDisabled = ref(false);
const displayAsterisk = ref(true);
const required = ref(true);
const variant = ref('outlined');
const appendInnerIcon = ref('');
const showAppendSlot = ref(false);
const eventLogs = ref<string[]>([]);

const validationRules = computed(() => {
  const rules = [];

  if (required.value) {
    rules.push((v: string) => !!v || 'Ce champ est requis');
  }

  rules.push((v: string) => v.length <= 20 || 'Maximum 20 caractères');

  return rules;
});

function handleBlur(event: FocusEvent) {
  eventLogs.value.push(`Blur: ${new Date().toLocaleTimeString()}`);
}

function handleInput(value: string) {
  eventLogs.value.push(`Input: ${value} à ${new Date().toLocaleTimeString()}`);
}

function clearLogs() {
  eventLogs.value = [];
}
</script>

<style scoped>
.playground-container {
  padding: 20px;
  font-family: Arial, sans-serif;
}

h1, h3 {
  color: #333;
}

.controls {
  margin-bottom: 20px;
  padding: 15px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.control-row {
  display: flex;
  gap: 20px;
  margin-bottom: 10px;
}

label {
  display: flex;
  align-items: center;
  gap: 5px;
}

.textfield-container {
  max-width: 500px;
  margin: 20px 0;
}

.events-log {
  margin-top: 20px;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.log-container {
  height: 150px;
  overflow-y: auto;
  padding: 10px;
  background-color: #f5f5f5;
  border-radius: 4px;
  margin-bottom: 10px;
}

.log-item {
  border-bottom: 1px solid #ddd;
  padding: 5px 0;
}

button {
  padding: 5px 10px;
  background-color: #f0f0f0;
  border: 1px solid #ddd;
  border-radius: 4px;
  cursor: pointer;
}

.value-display {
  margin-top: 20px;
}

pre {
  background-color: #f5f5f5;
  padding: 10px;
  border-radius: 4px;
  overflow-x: auto;
}
</style>
