<template>
  <div class="playground-container">
    <h1>PasswordField Playground</h1>

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
        <label>
          <input type="checkbox" v-model="outlined">
          Outlined
        </label>
      </div>

      <div class="control-row">
        <label>
          <span>Complexité minimum:</span>
          <select v-model="passwordComplexity">
            <option value="low">Faible</option>
            <option value="medium">Moyenne</option>
            <option value="high">Élevée</option>
          </select>
        </label>
        <label>
          <span>Visibilité du mot de passe:</span>
          <select v-model="passwordVisibility">
            <option value="toggle">Avec toggle</option>
            <option value="always">Toujours visible</option>
            <option value="never">Jamais visible</option>
          </select>
        </label>
      </div>

      <div class="control-row">
        <label>
          <span>Texte du label:</span>
          <input type="text" v-model="labelText">
        </label>
      </div>
    </div>

    <div class="passwordfield-container">
      <PasswordField
          v-model="passwordValue"
          :label="labelText"
          :required="required"
          :outlined="outlined"
          :display-asterisk="displayAsterisk"
          :is-validated-on-blur="isValidatedOnBlur"
          :is-disabled="isDisabled"
          :is-dirty="isDirty"
          :complexity="passwordComplexity"
          :password-visibility="passwordVisibility"
          @blur="handleBlur"
          @input="handleInput"
          @error="handleError"
      />
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
      <pre>{{ maskedPasswordValue }}</pre>
    </div>

    <div class="validation-info">
      <h3>Règles de validation</h3>
      <div v-if="passwordComplexity === 'low'">
        <strong>Faible:</strong> Minimum 8 caractères
      </div>
      <div v-if="passwordComplexity === 'medium'">
        <strong>Moyenne:</strong> Minimum 8 caractères, au moins 1 lettre et 1 chiffre
      </div>
      <div v-if="passwordComplexity === 'high'">
        <strong>Élevée:</strong> Minimum 8 caractères, au moins 1 majuscule, 1 minuscule, 1 chiffre et 1 caractère spécial
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import PasswordField from '@/components/PasswordField/PasswordField.vue';

const passwordValue = ref('');
const isValidatedOnBlur = ref(true);
const isDirty = ref(false);
const isDisabled = ref(false);
const displayAsterisk = ref(true);
const required = ref(true);
const outlined = ref(true);
const passwordComplexity = ref('medium');
const passwordVisibility = ref('toggle');
const labelText = ref('Mot de passe');
const eventLogs = ref<string[]>([]);

const maskedPasswordValue = computed(() => {
  if (passwordVisibility.value === 'always') {
    return passwordValue.value;
  }
  return passwordValue.value ? '•'.repeat(passwordValue.value.length) : '';
});

function handleBlur(event: FocusEvent) {
  eventLogs.value.push(`Blur: ${new Date().toLocaleTimeString()}`);
}

function handleInput(value: string) {
  eventLogs.value.push(`Input: Mot de passe modifié (${value.length} caractères) à ${new Date().toLocaleTimeString()}`);
}

function handleError(error: string) {
  eventLogs.value.push(`Erreur: ${error} à ${new Date().toLocaleTimeString()}`);
}

function clearLogs() {
  eventLogs.value = [];
}
</script>

<style scoped>
.playground-container {
  padding: 20px;
  font-family: Arial, sans-serif;
  max-width: 800px;
  margin: 0 auto;
}

h1, h3 {
  color: #333;
}

.controls {
  margin-bottom: 20px;
  padding: 15px;
  border: 1px solid #ddd;
  border-radius: 4px;
  background-color: #f9f9f9;
}

.control-row {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  margin-bottom: 10px;
}

label {
  display: flex;
  align-items: center;
  gap: 5px;
}

label span {
  min-width: 100px;
}

input[type="text"] {
  padding: 5px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

select {
  padding: 5px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.passwordfield-container {
  max-width: 500px;
  margin: 20px 0;
  padding: 15px;
  border: 1px solid #ddd;
  border-radius: 4px;
  background-color: white;
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
  font-family: monospace;
}

button {
  padding: 8px 15px;
  background-color: #f0f0f0;
  border: 1px solid #ddd;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #e0e0e0;
}

.value-display {
  margin-top: 20px;
}

pre {
  background-color: #f5f5f5;
  padding: 10px;
  border-radius: 4px;
  overflow-x: auto;
  font-family: monospace;
}

.validation-info {
  margin-top: 20px;
  padding: 15px;
  background-color: #e7f3ff;
  border-left: 4px solid #1976d2;
  border-radius: 4px;
}
</style>
