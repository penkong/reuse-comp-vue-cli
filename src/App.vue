<template>

    <SafaComboBox
      :m="m"
      :c="c"
      :align="align"
      :height="height"
      :width="width"
      :value="value"
      :label="label"
      :dense="dense"
      :placeholder="placeholder"
      :helper="helper"
      :items="items"
      :DtoName="DtoName"
      :Domain="Domain"
      :HideValue="HideValue"
      @inputer="checkInfo"
      @selectedVal="selectedVal"
    />
</template>

<script>
import SafaComboBox from "./components/SafaComboBox/SafaComboBox";
export default {
  name: "PageIndex",
  components: {
    SafaComboBox
  },
  data() {
    return {
      m: "e",
      c: "2",
      align: "right",
      width: "150px",
      height: "300px",
      value: "",
      label: "نظرات",
      dense: true,
      placeholder: "انتخاب",
      helper: "تصحیح شود",
      items: [
        // sequence is important
        // wrong { id , title}
        // correct {title , id}  => will become {label,code}
        { title: "ایران", id: "1" },
        { title: "کانادا", id: "2" },
        { title: "کره شمالی", id: "3" },
        { title: "کره جنوبی", id: "4" },
        { title: "آمریکا", id: "5" },
        { title: "کاستاریکا", id: "6" },
        { title: "جزایر مارشال", id: "7" },
        { title: "گوام", id: "8" },
        { title: "بنگال", id: "9" },
        { title: "تاسمانی", id: "10" },
        { title: "چاد", id: "11" },
        { title: "ساحل عاج", id: "12" }
      ],
      DtoName: "",
      Domain: "",
      HideValue: true
    };
  },
  methods: {
    checkInfo(val) {
      console.log(val, "i am checkInfo");
    },
    selectedVal(p) {
      console.log(p, "i am selected val");
    }
  }
};
</script>
