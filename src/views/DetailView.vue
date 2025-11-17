<template>
  <div class="detail-container">
    
      <!-- Columna esquerra -->
      <div class="left-column">

        <div class="form-group">
           <label>Moto Id:</label>
           <span class="value">{{ moto.id }}</span>
        </div>

        <div class="form-group">
          <label>Custom Id:</label>
          <input v-model="editable.customId" type="text" />
        </div>

        <div class="form-group">
          <label>Plate:</label>
          <input v-model="editable.plate" type="text" />
        </div>

        <div class="form-group">
          <label>Type:</label>
          <span>{{ moto.type }}</span>
        </div>

        <div class="form-group">
          <label>Model:</label>
          <span>{{ moto.model }}</span>
        </div>

        <div class="form-group checkbox">
          <label>Has Special Trunk</label>
          <input v-model="editable.hasSpecialTrunk" type="checkbox" /> 
        </div>

        <div class="form-group checkbox">
          <label>Has Promo</label>
          <input v-model="editable.hasPromo" type="checkbox" /> 
        </div>

        <div class="form-group checkbox"> 
          <label>Has Helmet Sensor</label>
          <input v-model="editable.hasHelmetSensor" type="checkbox" /> 
        </div>

        <div class="button-group">
          <button class="btn-save" @click="saveChanges">Save / Update</button>
          <button class="btn-discard" @click="discardChanges"> Discard</button>
        </div>
      </div>

      <!-- Columna dreta -->
      <div class="right-column">

         <div class="form-group">
           <label>Battery Level: </label>
           <span>{{ formatBattery(moto.batteryLevel) }}</span>
         </div>

         <div class="form-group">
           <label>Autonomy: </label>
           <span>{{ moto.autonomy }} Km</span>
         </div>

        <div class="map-container">
          <div class="form-group location"> 
            <label>Location </label>
            
            <div class="map-frame">
                <iframe
                  v-if="moto.position"
                  :src="mapUrl"
                  width="100%"
                  height="320"
                  style="border:0;"
                  loading="lazy"
                ></iframe>
            </div>
          </div>
        </div>
      </div>
  </div>
</template>

<script>
import motos from '../data/demo_vehicles.json'
import { ref, computed } from 'vue'
import { useRoute, useRouter } from 'vue-router'

export default {
  setup() {
    const route = useRoute()
    const router = useRouter()
    const id = Number(route.params.id)
    const moto = motos.find((m) => m.id === id)

    const editable = ref({
      customId: moto.customId,
      plate: moto.plate,
      hasPromo: moto.hasPromo,
      hasSpecialTrunk: moto.hasSpecialTrunk,
      hasHelmetSensor: moto.hasHelmetSensor
    })

    const formatBattery = (val) => {
      if (typeof val === 'number') return Math.round(val * 1) + '%'
      return val
    }

    const goHome = () => router.push('/')

    const discardChanges = () => {
      editable.value = {
        customId: moto.customId,
        plate: moto.plate,
        hasPromo: moto.hasPromo,
        hasSpecialTrunk: moto.hasSpecialTrunk,
        hasHelmetSensor: moto.hasHelmetSensor
      }
    }

    const saveChanges = () => {
      Object.assign(moto, editable.value)
      alert('Updated Data')
    }

    const mapUrl = computed(() => {
      if (!moto.position) return ''
      const { lat, lng } = moto.position
      return `https://www.google.com/maps?q=${lat},${lng}&z=15&output=embed`
    })

    return { moto, editable, formatBattery, goHome, discardChanges, saveChanges, mapUrl }
  }
}
</script>


<style scoped>
.detail-container {
  display: flex;
  gap: 60px;
  justify-content: space-between;
  align-items: flex-start;
  padding:100px 60px 10px 60px;
  height: calc(100vh - 100px);
  box-sizing: border-box; 
}

.left-column, .right-column {
  display: flex;
  flex-direction: column;
  width: 40%;
}

.right-column {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
}

.form-group {
  padding-bottom: 24px;
  font-size: 16px;
  display: flex;
  align-items: center;
}

.form-group label {
  font-weight:bold;
  color: #333;
  width: 220px;
  text-align: left;
}

.form-group span,
.form-group input[type="text"] {
  flex: 1; 
  color: #555;
}

.form-group input[type="text"] {
  padding: 6px 10px;
  border: 1px solid #ccc;
  border-radius: 6px;
  width: 100%;
  max-width: 200px;
}

.form-group.checkbox {
  display: flex;
  align-items: center;
}

.form-group.checkbox label {
  display: flex;
  align-items: center;
  font-weight: bold;
  width: 220px; 
}

.button-group {
  display: flex;
  gap: 80px;
  margin-top:80px;
  justify-content: flex-start;
}

.btn-save, .btn-discard {
  min-width: 80px;
  width: 180px;
  border:none;
  padding: 10px;
  border-radius:3px;
  font-size: 16px;
  font-family: inherit;
  background-color: #697798;
  color: white;
  cursor: pointer;
}

.btn-save:hover, .btn-discard:hover {
  background-color: #cbac4f;
}

.map-container {
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  width: 100%;  
}

.form-group.location {
  flex-direction: column;
  align-items: flex-start;
  padding-bottom: 0px;
}

.form-group.location label {
  margin-bottom: 8px;
}

.map-frame {
  width: 100%;
  height: 320px;
  border: 1px solid black;
  border-radius: 2px;
}
</style>
