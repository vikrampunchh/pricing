<template>
  <div class="container-fluid">
    <div class="container p-5">
      <div class="row">
        <div class="col-lg-4 col-md-12 mb-4" v-for="plan in plans" :key="plan.id">
          <div class="card h-100 shadow-lg" :class="selectedPlan.id == plan.id ? 'active' : ''">
            <div class="card-body">
              <div class="text-center p-3">
                <h5 class="card-title text-uppercase">{{ plan.name }}</h5>
                <small>{{ plan.desc }}</small>
                <br />
                <br />
              </div>
              <ul class="list-group list-group-flush">
                <li class="list-group-item"><span class="h5">$0.0007</span> Per Event (after {{ numberFormatting(plan.free_events) }})</li>
                <li class="list-group-item">
                  <span class="h5">${{ plan.base_price }}</span> Per Month
                </li>
                <li class="list-group-item">{{ plan.notes }}</li>
                <li class="list-group-item">
                  <span class="h5">${{ plan.base_price / plan.free_events }}</span> Per Event (for initial {{ numberFormatting(plan.free_events) }})
                </li>
              </ul>
            </div>
            <div class="card-body text-center">
              <button
                class="btn"
                :class="selectedPlan.id == plan.id ? 'btn-primary btn-lg' : 'btn-outline-primary btn-lg btn-selected'"
                @click="selectPlan(plan)"
              >
                Select
              </button>
            </div>
          </div>
        </div>
      </div>

      <div class="row">
        <div class="clearfix">
          <div class="card h-100 shadow active">
            <div class="card-body">
              <div class="text-center p-3">
                <h5 class="card-title text-uppercase">Pricing Calculation</h5>
              </div>
              <ul class="list-group list-group-flush">
                <li class="list-group-item d-flex justify-content-between">
                  <div>
                    <input
                      type="text"
                      placeholder="Number of events"
                      @input="eventsCountChange"
                      v-model="eventsCount"
                      class="form-control form-control-lg"
                    />
                  </div>
                  <div class="h5"></div>
                </li>
                <li class="list-group-item d-flex justify-content-between">
                  <div>
                    Base Price <br />
                    <small class="text-grey">Includes {{ numberFormatting(selectedPlan.free_events) }} events</small>
                  </div>
                  <div class="h6">${{ selectedPlan.base_price }}</div>
                </li>
                <li class="list-group-item d-flex justify-content-between">
                  <div>
                    After 1M <br />
                    <small class="text-grey">{{ plansRemainingEvents() }}</small>
                  </div>
                  <div class="h6">${{ remainingEventsCost() }}</div>
                </li>
                <li class="list-group-item d-flex justify-content-between">
                  <div>
                    Total <br />
                    <small class="text-grey"></small>
                  </div>
                  <div class="h4">${{ totalCost() }}</div>
                </li>
              </ul>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  name: 'HelloWorld',
  props: {
    msg: String,
  },
  setup() {
    const eventsCount = ref('')
    const plans = ref([
      {
        id: 1,
        name: 'Tier A - Entry Level',
        desc: 'Includes 400K events per month across all webhook/Amplify integrations',
        notes: 'Low barrier to entry/ Higher CPE* after initial allocation',
        base_price: 500,
        free_events: 400000,
        per_event: 0.0007,
      },
      {
        id: 2,
        name: 'Tier B - Moderate Usage',
        desc: 'Includes 1M events per month across all webhook/Amplify integrations',
        notes: 'Moderate barrier to entry/ Cost-effective CPE* after initial allocation',
        base_price: 1000,
        free_events: 1000000,
        per_event: 0.0004,
      },
      {
        id: 3,
        name: 'Tier C - Power User',
        desc: 'Includes 5M events per month across all webhook/Amplify integrations',
        notes: 'Enterprise level / Very low CPE* after initial allocation',
        base_price: 5000,
        free_events: 5000000,
        per_event: 0.0002,
      },
    ])
    const selectedPlan = ref(plans.value[1])

    const eventsCountChange = function (event) {
      const inputValue = event.target.value
      var events = inputValue.replace(/\D/g, '')
      if (parseInt(events)) {
        eventsCount.value = parseInt(events).toLocaleString('en-US')
      } else {
        eventsCount.value = ''
      }
    }

    const numberFormatting = function (num) {
      if (!num) return '0'

      var units = ['K', 'M', 'B', 'T', 'Q']
      var unit = Math.floor((num / 1.0e1).toFixed(0).toString().length)
      var r = unit % 3
      var x = Math.abs(Number(num)) / Number('1.0e+' + (unit - r)).toFixed(2)
      return x.toFixed(2) + '' + units[Math.floor(unit / 3) - 1]
    }

    const selectPlan = function (plan) {
      selectedPlan.value = plan
    }

    const plansRemainingEvents = function () {
      var events = eventsCount.value.replace(/\D/g, '')
      if (events && events > selectedPlan.value.free_events) {
        return `${parseInt(events) - selectedPlan.value.free_events}`
      }
      return ''
    }

    const remainingEventsCost = function () {
      const events = eventsCount.value.replace(/\D/g, '')
      if (events && events > selectedPlan.value.free_events) {
        const remainingEvents = parseInt(events) - selectedPlan.value.free_events
        return remainingEvents * selectedPlan.value.per_event
      }
      return 0
    }

    const totalCost = function () {
      return remainingEventsCost() + selectedPlan.value.base_price
    }

    return {
      plans,
      eventsCount,
      selectedPlan,
      eventsCountChange,
      numberFormatting,
      selectPlan,
      plansRemainingEvents,
      remainingEventsCost,
      totalCost,
    }
  },
}
</script>

<style scoped>
.card {
  border: none;
  padding: 10px 20px;
}

.card::after {
  position: absolute;
  z-index: -1;
  opacity: 0;
  -webkit-transition: all 0.6s cubic-bezier(0.165, 0.84, 0.44, 1);
  transition: all 0.6s cubic-bezier(0.165, 0.84, 0.44, 1);
}

.card:hover,
.card.active {
  transform: scale(1.02, 1.02);
  -webkit-transform: scale(1.02, 1.02);
  backface-visibility: hidden;
  will-change: transform;
  box-shadow: 0 0rem 3rem rgba(0, 0, 0, 0.5) !important;
}

.card:hover::after {
  opacity: 1;
}

.card:hover .btn-outline-primary {
  color: white;
  background: #007bff;
}
</style>
