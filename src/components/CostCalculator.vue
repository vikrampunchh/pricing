<template>
  <div class="container-fluid">
    <div class="container p-5">
      <div class="row">
        <div class="col-lg-4 col-md-12 mb-4">
          <div class="card h-100 shadow-lg">
            <div class="card-body">
              <div class="text-center p-3">
                <h5 class="card-title text-uppercase">TIER A - Entry Level</h5>
                <small>Includes 400K events per month across all webhook/Amplify integrations</small>
                <br />
                <br />
              </div>
              <ul class="list-group list-group-flush">
                <li class="list-group-item"><span class="h5">$0.0007</span> Event (after 400K)</li>
                <li class="list-group-item"><span class="h5">$500</span> Per Month</li>
                <li class="list-group-item">Low barrier to entry/Higher CPE* after initial allocation</li>
                <li class="list-group-item"><span class="h5">$0.00125</span> Per Event (for initial 400K)</li>
              </ul>
            </div>
            <div class="card-body text-center">
              <button class="btn btn-outline-primary btn-lg btn-selected">Select</button>
            </div>
          </div>
        </div>
        <div class="col-lg-4 col-md-12 mb-4">
          <div class="card h-100 shadow-lg active">
            <div class="card-body">
              <div class="text-center p-3">
                <h5 class="card-title text-uppercase">Tier B - Moderate Usage</h5>
                <small>Includes 1M events per month across all webhook/Amplify integrations</small>
                <br />
                <br />
              </div>
              <ul class="list-group list-group-flush">
                <li class="list-group-item"><span class="h5">$0.0007</span> Event (after 400K)</li>
                <li class="list-group-item"><span class="h5">$500</span> Per Month</li>
                <li class="list-group-item">Low barrier to entry/Higher CPE* after initial allocation</li>
                <li class="list-group-item"><span class="h5">$0.00125</span> Per Event (for initial 400K)</li>
              </ul>
            </div>
            <div class="card-body text-center">
              <button class="btn btn-primary btn-lg">Select</button>
            </div>
          </div>
        </div>
        <div class="col-lg-4 col-md-12 mb-4">
          <div class="card h-100 shadow-lg">
            <div class="card-body">
              <div class="text-center p-3">
                <h5 class="card-title text-uppercase">Tier C - Power User</h5>
                <small>Includes 5M events per month across all webhook/Amplify integrations</small>
                <br />
                <br />
              </div>
              <ul class="list-group list-group-flush">
                <li class="list-group-item"><span class="h5">$5000</span> Per Month</li>
                <li class="list-group-item"><span class="h5">$0.0007</span> Event (after 400K)</li>
                <li class="list-group-item">Low barrier to entry/Higher CPE* after initial allocation</li>
                <li class="list-group-item"><span class="h5">$0.00125</span> Per Event (for initial 400K)</li>
              </ul>
            </div>
            <div class="card-body text-center">
              <button class="btn btn-outline-primary btn-lg">Select</button>
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
                  <div class="h5">$0.0007</div>
                </li>
                <li class="list-group-item d-flex justify-content-between">
                  <div>
                    Base Price <br />
                    <small class="text-grey">Includes 1M events</small>
                  </div>
                  <div class="h6">$0.0007</div>
                </li>
                <li class="list-group-item d-flex justify-content-between">
                  <div>
                    After 1M <br />
                    <small class="text-grey">Includes 1M events</small>
                  </div>
                  <div class="h6">$0.0007</div>
                </li>
                <li class="list-group-item d-flex justify-content-between">
                  <div>
                    Total <br />
                    <small class="text-grey">Includes 1M events</small>
                  </div>
                  <div class="h4">$0.0007</div>
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
import '@/assets/styles.css'

export default {
  name: 'HelloWorld',
  props: {
    msg: String,
  },
  setup() {
    const eventsCount = ref('')
    const eventsCountChange = function (event) {
      const inputValue = event.target.value
      console.log('Input value changed:', inputValue, event)
      var events = inputValue.replace(/\D/g, '')

      if (parseInt(events)) {
        eventsCount.value = parseInt(events).toLocaleString('en-US')
      } else {
        eventsCount.value = ''
      }
    }

    return {
      eventsCount,
      eventsCountChange,
    }
  },
}
</script>

<style scoped></style>
