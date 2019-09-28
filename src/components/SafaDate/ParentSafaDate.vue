<template>
  <SafaDate
    :align="align"
    :m="m"
    :type="type"
    :value="value"
    :label="label"
    :icon="icon"
    :helper="helper"
    :errorlabel="errorlabel"
    :c="c"
    @inputer="logger"
  ></SafaDate>
</template>

<style></style>

<script>
// npm i --save @fortawesome/vue-fontawesome
//  npm i --save @fortawesome/free-solid-svg-icons
// $ npm i --save @fortawesome/fontawesome
//  npm i --save @fortawesome/fontawesome-svg-core
// $ $ npm i --save @fortawesome/free-solid-svg-icons
// $ npm i vue-persian-datetime-picker
import SafaDate from "./components/SafaDate/SafaDate";
export default {
  name: "PageIndex",
  components: {
    SafaDate
  },
  data() {
    return {
      align: "right",
      m: "e",
      minDate: "",
      maxDate: "",
      // for type date or datatime
      type: "text",
      value: "1398/01/01",
      label: "زمان قرارداد",
      icon: "today",
      helper: "شما اینجایی",
      errorlabel: " الزامی",
      c: "5"
    };
  },
  methods: {
    logger(val) {
      console.log(val);
    }
  }
};
</script>
