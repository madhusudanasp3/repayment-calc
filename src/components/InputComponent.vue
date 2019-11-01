<template>
  <div>
    <div class="row">
      <div class="col">
        <div class="row">
          <div class="col">
            <div class="row padding-label font-weight-bold padding-left--text">
              <span>How To Use</span>
            </div>
            <div class="row padding-left--text">
              <p>The repayment plan calculator is provided to help you with general information regarding options to help you catch up on past due payments. The results returned by this calculator should only be used as one of many factors in evaluating your options.</p>
              <p>This calculator returns information based on inputs regarding your existing mortgage information. It is important that you provide accurate information in order to receive more realistic results. This calculator can only provide a general overview of your situation based on the information you provide. Your mortgage company may use different information to determine eligibility and your individual results may vary from the results shown by this calculator.</p>
            </div>
          </div>
        </div>
        <form id="form" @submit="submitForm">
          <div class="row">
            <div class="col">
              <div
                class="row padding-label color--blue font-weight-bold background--gray background--rounded margin--bottom"
              >
                <span>Your Existing First Mortgage Information</span>
              </div>
            </div>
          </div>
          <InputFieldComponent
            :inputField="mortgageInfo.monthlyPaymentAmount"
            :isInvalid="!mortgageInfo.monthlyPaymentAmount.isValid"
            @enter-value="validateGreaterThanZero"
          ></InputFieldComponent>
          <div
            class="form-group row background--gray background--rounded margin--bottom align-items-center"
          >
            <label
              for="numberOfPayments"
              class="col-sm-6 padding-right col-form-label font-weight-bold"
            >Number Of Payments Past Due</label>
            <div class="col-sm-6 padding-right-none">
              <div class="input-group">
                <input
                  type="text"
                  class="form-control rounded-right"
                  id="numberOfPayments"
                  placeholder="Enter Numer of Payments"
                  v-model="mortgageInfo.numberOfPayments.value"
                  v-bind:class="{ 'is-invalid': !mortgageInfo.numberOfPayments.isValid }"
                  @input="validateNumberOfPaymanets"
                  @change="calculatePaymentPastDue"
                />
                <div
                  class="invalid-feedback"
                >This field is optional, but if you enter it must be numeric or leave it blank.</div>
              </div>
            </div>
          </div>

          <InputFieldComponent
            :inputField="mortgageInfo.totalAmountPastDue"
            :isInvalid="!mortgageInfo.totalAmountPastDue.isValid"
            @enter-value="validateGreaterThanZero"
          ></InputFieldComponent>

          <div
            class="form-group row background--gray background--rounded margin--bottom align-items-center"
          >
            <label class="col-sm-6 padding-right-none col-form-label font-weight-bold">
              Number of Months for Repayment Plan
              <br />
              <span class="desc">(Usually 3-9 months, 6 months used for this example.)</span>
            </label>
            <div class="col-sm-6 padding-right-none">
              <div class="input-group">
                <span
                  class="form-control--plaintext font-weight-bold"
                >{{ this.repaymentPlan + ' months'}}</span>
              </div>
            </div>
          </div>
          <div class="form-group row">
            <div class="col">
              <div class="row">
                <button class="btn btn-primary font-weight-bold">Calculate Results</button>
              </div>
            </div>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import InputFieldComponent from "./InputFieldComponent";

export default {
  name: "InputComponent",
  components: {
    InputFieldComponent
  },
  data() {
    return {
      repaymentPlan: 6,
      mortgageInfo: {
        monthlyPaymentAmount: {
          id: "monthlyPaymentAmount",
          value: null,
          isValid: true,
          label: "Monthly Payment Amount",
          icon: "$",
          type: "text",
          placeHolder: "Enter Amount",
          errorMsg:
            "This field is required and must be numeric and greater than zero."
        },
        numberOfPayments: {
          id: "numberOfPayments",
          value: null,
          isValid: true,
          label: "Number Of Payments Past Due",
          icon: "",
          type: "text",
          placeHolder: "Enter Numeber of Payments",
          errorMsg: "This field is required and must be numeric."
        },
        totalAmountPastDue: {
          id: "totalAmountPastDue",
          value: null,
          isValid: true,
          label: "Or, Total Dollar Amount Past Due",
          icon: "$",
          type: "text",
          placeHolder: "Enter Amount",
          errorMsg: "This field is required and must be numeric."
        }
      },
      repaymentInfo: {
        monthlyFirstMorgageBalance: null,
        pastDueAmount: null,
        numberOfMonthsToRepay: null,
        newMortagaePayment: null,
        additionalAmount: null
      }
    };
  },
  computed: {},
  methods: {
    calculatePaymentPastDue() {
      if (this.mortgageInfo.numberOfPayments.value != "0")
        this.mortgageInfo.totalAmountPastDue.value =
          this.mortgageInfo.monthlyPaymentAmount.value *
          this.mortgageInfo.numberOfPayments.value;
    },
    validateNumberOfPaymanets() {
      this.validateOptionalField(this.mortgageInfo.numberOfPayments);
    },
    validateOptionalField(input) {
      if (this.isFieldOptional(input.value)) {
        input.isValid = true;
      } else {
        input.isValid = this.isNumeric(input.value);
      }
    },

    validateGreaterThanZero(input) {
      input.isValid = this.isValid(input.value) && parseFloat(input.value) > 0;
    },
    validate(input) {
      input.isValid = this.isValid(input.value) && parseFloat(input.value) >= 0;
    },

    isFieldOptional(n) {
      return n == null || this.isEmpty(n);
    },
    isValid(n) {
      // replace with regex
      return !this.isEmpty(n) && this.isNumeric(n);
    },
    isEmpty(n) {
      return n === "";
    },
    isNumeric(n) {
      // eslint-disable-next-line no-restricted-globals
      return !isNaN(parseFloat(n)) && isFinite(n);
    },
    submitForm(event) {
      if (!this.isValidForm(this.mortgageInfo)) {
        event.preventDefault();
        return;
      }
      this.repaymentInfo.monthlyFirstMorgageBalance = this.mortgageInfo.monthlyPaymentAmount.value;
      this.repaymentInfo.pastDueAmount = this.mortgageInfo.totalAmountPastDue.value;
      this.repaymentInfo.numberOfMonthsToRepay = this.repaymentPlan;
      this.repaymentInfo.newMortagaePayment =
        parseFloat(this.mortgageInfo.monthlyPaymentAmount.value) +
        this.mortgageInfo.totalAmountPastDue.value / 6;
      this.repaymentInfo.additionalAmount =
        this.mortgageInfo.totalAmountPastDue.value / 6;

      this.$emit("submit-form", this.repaymentInfo);

      event.preventDefault();
    },
    formatNumber(num) {
      return Number(num).toLocaleString(undefined, {
        minimumFractionDigits: 2,
        maximumFractionDigits: 2
      });
    },
    isValidForm(mortgageInfo) {
      let isFormValid = true;
      this.validateGreaterThanZero(mortgageInfo.monthlyPaymentAmount);
      this.validateOptionalField(mortgageInfo.numberOfPayments);
      this.validateGreaterThanZero(mortgageInfo.totalAmountPastDue);

      isFormValid =
        mortgageInfo.monthlyPaymentAmount.isValid &&
        mortgageInfo.numberOfPayments.isValid &&
        mortgageInfo.totalAmountPastDue.isValid;

      return isFormValid;
    }
  }
};
</script>

<style lang="scss" scoped>
</style>