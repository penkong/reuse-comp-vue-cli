<template>
  <div class="safa-data">
    <button
      :loading="loadingGear"
      :disabled="read"
      @click="show=true"
      @click.prevent="simulateProgress"
      type="button"
      icon="today"
      :style="{
        marginLeft: '1rem',
        width: '1.5rem',
        height: '1.5rem',
        border: 'none',
        outline: 'none',
        cursor: 'pointer',
        borderRadius: '50%',
        backgroundColor: '#297af2',
      }"
    >
      <font-awesome-icon icon="calendar-day" style="color: white; outline: none;" />
      <date-picker
        element="my-custom-editable-input"
        :editable="read"
        :disabled="read"
        :v-model="type"
        :show="show"
        :min="minDate"
        :max="maxDate"
        :highlight="highlight"
        :auto-submit="true"
        @input="handleInput($event)"
        @close="show=false"
        locale="fa"
        color="#297af2"
        inputFormat="YYYY-MM-DD HH:mm"
        format="jYYYY - jMM - jDD  --  HH:mm"
        type="datetime"
        appendTo="body"
      />
    </button>
    <input
      id="my-custom-editable-input"
      :disable="read"
      :placeholder="value"
      :hint="hinter()"
      v-model="datetime"
      type="text"
      :style="{
        marginLeft: '1rem',
        border: 'none',
        borderRadius: '3px',
        backgroundColor: '#edf4ff',
        padding: '3px 4px 3px 4px'
      }"
    />
  </div>
</template>

<script>
import VuePersianDatetimePicker from "vue-persian-datetime-picker";
import uid from "uuid/v4";
export default {
  name: "SafaDate",
  components: {
    DatePicker: VuePersianDatetimePicker
  },
  data() {
    return {
      idOfInput: null,
      date: "",
      datetime: "",
      initHelper: null,
      show: false,
      loadingGear: false,
      read: null,
      notEditable: null,
      dense: true
    };
  },
  props: {
    align: {
      type: String,
      required: true,
      default: "right",
      validator: v => ["right", "left"].includes(v)
    },
    m: {
      type: String,
      validator: v => ["r", "e", "ne"].includes(v),
      default: "r"
    },
    minDate: {
      type: String,
      default: ""
    },
    maxDate: {
      type: String,
      default: ""
    },
    type: {
      type: String,
      required: true,
      default: "datetime"
    },
    value: { type: String },
    label: { default: "none", type: String },
    icon: {
      type: String,
      default: "today",
      required: true
    },
    helper: { type: String, default: "شما اینجایی" },
    errorlabel: { type: String, default: "تصحیح شود" },
    objColor: {
      type: Object
    },
    c: {
      type: String,
      validator: v =>
        [
          "1",
          "2",
          "3",
          "4",
          "5",
          "6",
          "7",
          "8",
          "9",
          "10",
          "11",
          "12",
          "auto"
        ].includes(v),
      default: "4"
    }
  },
  created() {
    const generatedId = "Safa" + "_" + uid();
    this.$set(this, "idOfInput", generatedId);
    switch (this.m) {
      case "r":
        return this.$set(this, "read", true);
      case "ne":
        return this.$set(this, "notEditable", true);
      default:
        return;
    }
  },
  methods: {
    hinter() {
      if (this.read) {
        return this.$set(this, "initHelper", null);
      } else {
        return this.$set(this, "initHelper", this.helper);
      }
    },
    handleInput($event) {
      this.$set(this, "datetime", $event);
      return this.$emit("inputer", this.datetime);
    },
    simulateProgress() {
      // we set loading state
      this.loadingGear = true;
      // simulate a delay
      setTimeout(() => {
        // we're done, we reset loading state
        this.loadingGear = false;
      }, 500);
    },
    highlight() {
      return {
        style: {
          color: "grey-3",
          "font-family": "'behdad' ,'Courier New' ,'Courier', 'monospace'"
        }
      };
    }
  }
};
</script>

<style lang="scss" scoped>
@import url("http://cdn.font-store.ir/behdad.css");
.for-input {
  display: inline-block;
  border: none;
  padding: 0.35rem;
  width: 90%;
}
</style>
