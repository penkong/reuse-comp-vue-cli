<template>
  <SafaInput
    :align="align"
    :m="m"
    :type="type"
    :value="value"
    :label="label"
    :placeholder="placeholder"
    :helper="helper"
    :maxChar="maxChar"
    :widthOfInput="widthOfInput"
    :widthOfRow="widthOfRow"
    :errorlabel="errorlabel"
    :icon="icon"
    :colored="color"
    :c="c"
    :minChar="minChar"
    @inputer="checkVal"
  />
</template>

<style></style>

<script>
import SafaInput from "../components/SafaInput/SafaInput";
export default {
  name: "PageIndex",
  components: {
    SafaInput
  },
  data() {
    return {
      align: "right",
      m: "e",
      type: "text",
      value: "",
      label: "نام",
      placeholder: "ورود اطلاعات",
      helper: "شما اینجایی",
      errorlabel: " الزامی",
      maxChar: 65,
      minChar: 3,
      // it must be base on quasar icon name
      icon: "event",
      color: "rgb(236, 236, 236)",
      //rem
      widthOfRow: "13",
      widthOfInput: "12",
      c: "1"
    };
  },
  methods: {
    checkVal(val) {
      console.log(val);
    }
  }
};
</script>
