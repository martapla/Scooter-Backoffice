<template>
  <div class="list-container">
    <h2>Scooter List</h2>

    <table class="table">
      <thead>
        <tr>
          <th>Id
            <input
                v-model="filterId"
                type="text"
                placeholder="Search 🔍"
                class="filter-input"
            />
          </th>
          <th>CustomId</th>
          <th>Plate
            <input
                v-model="filterPlate"
                type="text"
                placeholder="Search 🔍"
                class="filter-input"
            />
          </th>
          <th>
            <div class="sort-container">
                 Battery Level
                <div class="sort-arrows">
                    <div  
                    class="arrow"
                    :class="{ active: sortField === 'batteryLevel' && sortDirection === 'asc' }"
                    @click="setSort('batteryLevel', 'asc')"
                    >▲</div>
                    <div 
                    class="arrow"
                    :class="{ active: sortField === 'batteryLevel' && sortDirection === 'desc' }"
                    @click="setSort('batteryLevel', 'desc')"
                    >▼</div>
                </div>
            </div>
          </th>

          <th>
            <div class="sort-container">
                 Autonomy
                <div class="sort-arrows">
                    <div  
                        class="arrow"
                        :class="{ active: sortField === 'autonomy' && sortDirection === 'asc' }"
                        @click="setSort('autonomy', 'asc')"
                        >▲
                    </div>

                    <div 
                        class="arrow"
                        :class="{ active: sortField === 'autonomy' && sortDirection === 'desc' }"
                        @click="setSort('autonomy', 'desc')"
                    >▼
                    </div>
                </div>
            </div>
          </th>
          <th>hasPromo</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="m in paginated" :key="m.id" @click="openDetail(m.id)" style="cursor:pointer">
          <td>{{ m.id }}</td>
          <td>{{ m.customId }}</td>
          <td>{{ m.plate }}</td>
          <td>{{ formatBattery(m.batteryLevel) }}</td>
          <td>{{ m.autonomy }} Km.</td>
          <td>{{ m.hasPromo ? '✓' : '×' }}</td>
        </tr>
      </tbody>
    </table>
  </div>
  
    <!-- Footer -->
  <div class="footer">

      <div class="pagination-controls">

        <label class="page-size">
          <select v-model.number="pageSize">
            <option :value="2">2</option>
            <option :value="3">3</option>
            <option :value="5">5</option>
          </select>
           Items per page
        </label>

        <div class="page-number">
          <p>Page</p>
          <span>{{ page }}</span>
          <div>
            <button class="footer-btn" :disabled="page===1" @click="page--">◀︎</button>
            <button class="footer-btn" :disabled="page===totalPages" @click="page++">▶︎</button>
          </div>
          <p>of</p>
          <span>{{ totalPages }}</span>
        </div>
      </div>
  </div>
  
</template>

<script>
import motos from '../data/demo_vehicles.json'
import { ref, computed, watch } from 'vue'
import { useRouter } from 'vue-router'

export default {
  setup() {
    const router = useRouter()
    const allMotos = ref(motos)

    // Paginació
    const page = ref(1)
    const pageSize = ref(3) // default  
    // Filtres
    const filterId = ref('')
    const filterPlate = ref('')
    // Ordenació
    const sortField = ref(null) // 'batteryLevel' | 'autonomy'
    const sortDirection = ref(null) // 'asc' | 'desc'

    const setSort = (field, direction) => {
      if (sortField.value === field && sortDirection.value === direction) {
        sortField.value = null
        sortDirection.value = null
      } else {
        sortField.value = field
        sortDirection.value = direction
      }
     
    }

    // --- Filtrat i ordenació ---
    const filtered = computed(() => {
      // 1) Filtrat per id i plate
      let result = allMotos.value.filter((m) => {
        const matchId = filterId.value
          ? m.id.toString().includes(filterId.value)
          : true
        const matchPlate = filterPlate.value
          ? m.plate.toLowerCase().includes(filterPlate.value.toLowerCase())
          : true
        return matchId && matchPlate
      })


      // 2️) Ordenació (aplicada sobre tot el conjunt)
      if (sortField.value && sortDirection.value) {
        result = [...result].sort((a, b) => {
          const A = a[sortField.value]
          const B = b[sortField.value]

          if (A < B) return sortDirection.value === 'asc' ? 1 : -1
          if (A > B) return sortDirection.value === 'asc' ? -1 : 1
          return 0
        })
      }

      return result
    })



    // -- Paginació --
    const totalPages = computed(() =>
        Math.max(1, Math.ceil(filtered.value.length / pageSize.value))
    )

    const startIndex = computed(() => (page.value - 1) * pageSize.value)
    const endIndex = computed(() =>
      Math.min(filtered.value.length, startIndex.value + pageSize.value)
    )

    const paginated = computed(() =>
      filtered.value.slice(startIndex.value, endIndex.value)
    )


    // --- Watchers ---
    watch([filterId, filterPlate, pageSize], () => (page.value = 1))

   

    // --- Helpers ---
    const formatBattery = (val) => {
      if (typeof val === 'number') return Math.round(val * 1) + '%'
      return val
    }

    const openDetail = (id) => {
      router.push({ name: 'Detail', params: { id } })
    }


    return {
      motos: allMotos,
      page, pageSize, totalPages,
      paginated, startIndex, endIndex,
      filterId,filterPlate, 
      sortField, sortDirection, setSort,
      formatBattery, openDetail
    }
  }
}
</script>

<style scoped>

.list-container {
  margin: 30px;
}

.table { 
  width:96%; 
  border-collapse: separate; 
  border-spacing: 0 14px; 
  margin:12px auto;
  table-layout: fixed;
}

.table th { 
  padding:8px 10px; 
  border:1px solid #9a9999; 
  text-align:left 
}

.table td { 
  padding: 12px 16px;
  text-align:left;
}

.table tbody tr {
  box-shadow: 0 0 0 1px #ddd;
  border-collapse: collapse;
}

.table tbody tr:hover {
  box-shadow: 0 0 0 1px  #b2b0b0;
  background: #eeeded;
  cursor: pointer;
}

.filter-input {
  width: 50%; 
  margin-left: 10px;  
  padding: 3px;         
  border: 1px solid #ccc;
  border-radius: 4px;
}

.sort-container {
  display: inline-flex;      
  align-items: center;      
  gap: 4px;               
}

.sort-arrows {
  display: flex;
  flex-direction: column; 
  align-items: center;
  justify-items: center;
  margin-left: 10px;
  line-height: 1;
}

.arrow {
  cursor: pointer;
  font-size: 16px;
  color:#697798;
  user-select: none;
}

.arrow:hover {
  color: #cbac4f;
}


/* Footer */
.footer {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background: white;
  border-top: 2px solid #697798;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 30px;
  z-index: 10; 
}

.pagination-controls { 
  position: relative;
  display:flex; 
  gap:20px; 
  align-items:center;
  width: 100%; 
}

.page-size select{ 
  margin: 0 12px ;
  padding:8px;
  cursor: pointer;
}

.footer-btn {
  border:none;
  padding: 0;
  font-size: 2.5em;
  font-family: inherit;
  background-color:transparent;
  color:  #567ba0;
  cursor: pointer;
}

.footer-btn:hover {
  color: #cbac4f;
}

.page-number { 
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  display:flex; 
  gap:8px; 
  align-items:center 
}

span { 
  border: 1px solid #ddd;
  padding: 12px 16px;
}

</style>
