<template>
  <div class="custom-dropdown-wrapper">
    <label for="location" class="form-label label">
      Location<span class="text-danger">*</span>
    </label>
    <div class="input-error-container" ref="dropdownRef" >
      <input v-model="selectedValue" @input="filterZones" @click.stop="toggleDropdown" @blur="validateLocation"  placeholder="Enter location..."
        class="form-control" />
      <span v-show="showError" class="error-message">Please choose location!</span>
    </div>
    <ul class="custom-dropdown" v-show="showDropdown">
      <li v-for="zone in filteredZones" :key="zone" @click="selectZone(zone)"
        :class="{ active: zone === selectedValue }" class="dropdown-item">
        {{ zone }}
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import moment from 'moment-timezone';

const emit = defineEmits();

const selectedValue = ref('');
const zones = moment.tz.names();
const filteredZones = ref(zones);
const showDropdown = ref(false);
const showError = ref(false);
const dropdownRef = ref(null);

const filterZones = () => {
  const term = selectedValue.value.toLowerCase();
  filteredZones.value = term
    ? zones.filter(z => z.toLowerCase().includes(term))
    : zones;
  showDropdown.value = true;
};

const selectZone = zone => {
  selectedValue.value = zone;
  showDropdown.value = false;
  showError.value = false;
  emit('update:modelValue', zone);
  emit('selectZone', zone);
};

const validateLocation = () => {
  showError.value = !selectedValue.value;
};

const toggleDropdown = () => showDropdown.value = !showDropdown.value;

const handleClickOutside = e => {
  if (dropdownRef.value && !dropdownRef.value.contains(e.target)) {
    showDropdown.value = false;
  };
};

onMounted(() => document.addEventListener('click', handleClickOutside));
</script>

<style scoped lang="scss">
.label {
  color: $classic-cream;
}

.custom-dropdown-wrapper {
  position: relative;

  .input-error-container {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    position: relative;

    .form-control {
      width: 100%;
    }

    .error-message {
      position: absolute;
      z-index: 1;
      top: 2.5rem;
      color: #dc3545;
      align-self: flex-end;
    }
  }

  .custom-dropdown {
    position: absolute;
    border: 1px solid #ccc;
    max-height: 150px;
    overflow-y: auto;
    z-index: 1000;
    background-color: white;
    width: 100%;
    padding: 0;
    border-radius: 10px;

    .dropdown-item {
      padding: 8px;
      cursor: pointer;

      &:hover {
        background-color: #f0f0f0;
      }
    }

    .active {
      background-color: #f0f0f0;
      color: #333;
    }
  }
}
</style>
