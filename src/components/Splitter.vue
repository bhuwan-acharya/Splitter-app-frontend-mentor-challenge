<template>
  <main>
    <div class="title-section">
      <h3 class="title">SPLI</h3>
      <h3 class="title">TTER</h3>
    </div>
    <section class="main-component">
      <article class="input-fields-section">
        <div class="bill input-section">
          <p class="input-label">Bill</p>
          <div class="input-icon-field">
            <i class="fa-solid fa-euro-sign icon"></i>
            <input
              type="number"
              oninput="this.value == 0  ? null : this.value = Math.abs(this.value)"
              class="input-field"
              placeholder="0"
              v-model.number="bill"
              min="0"
              step=".01"
              required
            />
          </div>
        </div>
        <div class="select-tip-component input-section">
          <p class="input-label">select Tip %</p>
          <div class="select-tips">
            <button
              :class="
                tipSelected == tip
                  ? 'tip-button tip-button-active'
                  : 'tip-button'
              "
              v-for="(tip, index) in tipArray"
              :key="index"
              :value="tip"
              name="tip"
              @click="(e) => selectTipHandle(e)"
            >
              {{ tip }}%
            </button>
            <input
              class="custom-tips-input"
              type="number"
              oninput="this.value == 0  ? null : this.value = Math.abs(this.value)"
              min="0"
              placeholder="Custom"
              :value="customTip"
              @input="(e) => handleCustomTip(e)"
              step=".01"
            />
          </div>
        </div>
        <div class="number-of-people">
          <div class="number-of-people-andno-zero-component">
            <p class="input-label">Number of People</p>
            <div v-if="numberOfPeople == 0 && numberOfPeople !== ''">
              <p style="color: red; line-height: 0px">Can't be zero</p>
            </div>
          </div>
          <div class="input-icon-field">
            <i class="fa-solid fa-users icon"></i>
            <input
              type="number"
              oninput="this.value == 0  ? null : this.value = Math.abs(this.value)"
              min="0"
              :class="
                numberOfPeople !== 0
                  ? 'input-field'
                  : 'input-field number-of-people-input-field'
              "
              placeholder="0"
              v-model="numberOfPeople"
              required
            />
          </div>
        </div>
      </article>
      <article class="total-display-component">
        <div class="tip-amount-component label-amount">
          <div class="tip-amount">
            <h3 class="label">Tip Amount</h3>
            <p class="person">/ person</p>
          </div>
          <div class="tip amount">
            ${{
              bill == null || bill == 0 || bill == NaN
                ? parseFloat(0).toFixed(2)
                : tipSelected && numberOfPeople
                ? (
                    (parseFloat(bill) * (parseFloat(tipSelected) / 100)) /
                    parseFloat(numberOfPeople)
                  ).toFixed(2)
                : parseFloat(0).toFixed(2)
            }}
          </div>
        </div>
        <div class="total-bill-per-person-component label-amount">
          <div class="total-bill-per-person">
            <h3>Total</h3>
            <p class="person">/ person</p>
          </div>
          <div class="total-bill amount">
            ${{
              bill == null || bill == 0
                ? parseFloat(0).toFixed(2)
                : tipSelected && numberOfPeople
                ? (
                    (parseFloat(bill) * (parseFloat(tipSelected) / 100) +
                      parseFloat(bill)) /
                    parseFloat(numberOfPeople)
                  ).toFixed(2)
                : parseFloat(0).toFixed(2)
            }}
          </div>
        </div>
        <div class="button-section">
          <button class="reset-btn" @click="onReset">RESET</button>
        </div>
      </article>
    </section>
  </main>
</template>

<script>
export default {
  name: "splitter-calculator",
  data() {
    return {
      tipArray: [5, 10, 15, 20, 25],
      bill: null,
      numberOfPeople: null,
      customTip: null,
      tipSelected: null,
    };
  },
  methods: {
    onReset: function () {
      this.bill = null;
      this.numberOfPeople = null;
      this.tipSelected = null;
    },
    selectTipHandle: function (e) {
      this.tipSelected = e.target.value;
    },
    handleCustomTip: function (e) {
      this.customTip = e.target.value;
      this.tipSelected = e.target.value;
    },
  },
};
</script>

<style>
/* this is body section */

body {
  background-color: hsl(185, 41%, 84%);
  font-family: "Space Mono", monospace;
  font-size: 1.5rem;
  color: hsl(183, 100%, 15%);
  text-transform: capitalize;
}

/* this is for whole component */
/* title section */
.title-section {
  letter-spacing: 0.75rem;
  margin-bottom: 4rem;
  margin-top: 4rem;
}
.title {
  letter-spacing: 0.75rem;
}
/* title section end */
/* main for title and component */
main {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  max-height: 100%;
}
/* main-component for calculator and display */
.main-component {
  background-color: #fff;
  width: 70vw;
  border-radius: 1.5rem;
  display: grid;
  grid-template-columns: 34vw 34vw;
  gap: 2rem;
  padding: 2rem;
}
/* section to display the final amounts */
.total-display-component {
  background-color: hsl(183, 100%, 15%);
  border-radius: 1rem;
  color: #fff;
  /* width: 100%; */
  /* height: 100%; */
  position: relative;
}
/* all the buttons section */
.input-fields-section {
  /* width: 100%; */
  padding: 1rem;
}
.input-icon-field i {
  position: absolute;
}
.icon {
  padding: 10px;
  color: hsl(184, 14%, 56%);
  min-width: 50px;
  text-align: center;
  margin-top: 0.75rem;
}
.input-field {
  width: 100%;
  padding: 0.5rem;
  border: none;
  background-color: hsl(189, 41%, 97%);
  color: hsl(183, 100%, 15%);
  text-align: right;
  font-size: 2.5rem;
  font-family: "Space Mono", monospace;
}
.input-label {
  margin-top: 0px;
  margin-bottom: 5px;
}
.input-section {
  margin-bottom: 3rem;
}
input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
input:focus {
  outline-color: hsl(172, 67%, 45%);
}

.number-of-people-input-field {
  outline-color: red;
}
.number-of-people-input-field:focus {
  outline-color: red;
}
/* for the tips button */
.select-tips {
  display: grid;
  grid-template-columns: 32% 32% 32%;
  gap: 1rem;
}
.tip-button {
  width: 100%;
  border-radius: 0.3rem;
  background-color: hsl(183, 100%, 15%);
  color: #fff;
  font-weight: 700;
  font-size: 1.25rem;
  padding: 1rem;
}
.tip-button:hover {
  background-color: hsl(185, 41%, 84%);
}
.tip-button-active {
  background-color: hsl(172, 67%, 45%);
}
button {
  font-family: "Space Mono", monospace;
}
.custom-tips-input {
  border: none;
  background-color: hsl(189, 41%, 97%);
  color: hsl(183, 100%, 15%);
  text-align: right;
  font-size: 1.5rem;
  font-weight: 700;
  text-transform: capitalize;
  text-align: center;
  font-family: "Space Mono", monospace;
}
::placeholder {
  color: hsl(186, 14%, 43%);
}
/* second article starts here */
.label-amount {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(80px, 1fr));
  margin: 2rem;
  align-items: center;
  line-height: 1rem;
  font-size: 1.25vmax;
}
.person {
  color: hsl(184, 14%, 56%);
  text-transform: lowercase;
}
.amount {
  font-size: 3.5vmax;
  font-weight: 700;
  color: hsl(172, 67%, 45%);
  text-align: right;
}
.reset-btn {
  width: 90%;
  padding: 1rem;
  font-size: 1.25vmax;
  font-weight: 700;
  background-color: hsl(172, 67%, 45%);
}
.button-section {
  display: flex;
  width: 100%;
  /* height: 100%; */
  justify-content: center;
  position: absolute;
  top: 80%;
}
.number-of-people-andno-zero-component {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
}
@media screen and (max-width: 375px) {
  .main-component {
    width: 100vw;
    display: flex;
    flex-direction: column;
    padding: 0rem;
  }
  .total-display-component {
    width: 83%;
    margin: 2rem;
  }
  .input-fields-section {
    padding: 2rem;
  }
  .select-tips {
    grid-template-columns: 48% 48%;
  }
  .reset-btn {
    width: 80%;
  }
  .button-section {
    position: static;
    margin-bottom: 2rem;
  }
  .number-of-people-andno-zero-component {
    flex-direction: column;
  }
}
</style>
