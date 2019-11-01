<template>
  <div>
    <div id="result" class="row margin--bottom">
      <div class="col">
        <div class="row padding-label font-weight-bold padding-left--text">
          <span class>Please Note:</span>
        </div>
        <div class="row padding-p padding-left--text">
          <p>These results have been calculated based on your inputs regarding your existing mortgage information. Your mortgage company may consider additional factors in determining your eligibility and potential savings, which may result in a different payment amount.</p>
          <p>Please save this information (as a PDF document) or email it to yourself, so that you may have it as a reference when you speak with your mortgage company or a housing counselor.</p>
          <i>Every situation is different. Contact your mortgage company to determine your exact results.</i>
          <br />
          <br />
        </div>
        <div class="row">
          <div class="col">
            <div
              class="row padding-label color--blue font-weight-bold background--gray background--rounded margin--bottom"
            >
              <span>Repayment Plan Information</span>
            </div>
          </div>
        </div>
        <div class="form-group row background--gray background--rounded padding margin--bottom">
          <label
            class="col-sm-6 padding-right-none col-form-label font-weight-bold"
          >Existing Monthly First Mortgage Payment</label>
          <div class="col-sm-6 padding-right-none">
            <div class="input-group">
              <span
                class="form-control-plaintext"
              >{{ '$ ' + formatNumber(result.monthlyFirstMorgageBalance) }}</span>
            </div>
          </div>
        </div>
        <div class="form-group row background--gray background--rounded padding margin--bottom">
          <label
            class="col-sm-6 padding-right-none col-form-label font-weight-bold"
          >Past Due Amount On Your First Mortgage</label>
          <div class="col-sm-6 padding-right-none">
            <div class="input-group">
              <span class="form-control-plaintext">{{'$ ' + formatNumber(result.pastDueAmount) }}</span>
            </div>
          </div>
        </div>
        <div class="form-group row background--gray background--rounded padding margin--bottom">
          <label class="col-sm-6 padding-right-none col-form-label font-weight-bold">
            Number Of Months To Repay Past Due Amount And Become Current
            <span
              class="desc"
            >(Other terms may be available.)</span>
          </label>
          <div class="col-sm-6 padding-right-none">
            <div class="input-group">
              <span class="form-control-plaintext">{{ result.numberOfMonthsToRepay }}</span>
            </div>
          </div>
        </div>
        <div class="form-group row background--gray background--rounded padding margin--bottom">
          <label
            class="col-sm-6 padding-right-none col-form-label font-weight-bold"
          >New Temporary Mortgage Payment</label>
          <div class="col-sm-6 padding-right-none">
            <div class="input-group">
              <span
                class="form-control-plaintext"
              >{{ '$ ' + formatNumber(result.newMortagaePayment) }}</span>
            </div>
          </div>
        </div>
        <div
          class="form-group row background--gray background--rounded padding margin--bottom align-items-center"
        >
          <label class="col-sm-6 padding-right-none col-form-label font-weight-bold">
            Additional Amount Paid Each Month
            <br />
          </label>
          <div class="col-sm-6 padding-right-none">
            <div class="input-group">
              <span class="form-control-plaintext">{{ '$ ' + formatNumber(result.additionalAmount)}}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ResultComponent",
  props: {
    result: Object
  },
  methods: {
    formatNumber(num) {
      return Number(num).toLocaleString(undefined, {
        minimumFractionDigits: 2,
        maximumFractionDigits: 2
      });
    }
  }
};
</script>

<style lang="scss">
</style>