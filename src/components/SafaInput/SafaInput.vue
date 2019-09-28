<template>
  <div :style="{maxWidth: `${widthOfRow}rem`}">
    <div v-if="aligned" class="aligned">
      <input
        :min="minChar"
        :max="maxChar"
        :value="value"
        @input="handleInput($event.target.value)"
        :disabled="read || notEditable"
        :type="type"
        :id="idOfInput"
        :name="idOfInput"
        v-model.lazy="text"
        :style="{
          marginRight: `${c}rem`,
          maxWidth: `${widthOfInput}rem`,
          'background-color': `${colored}`,
        }"
      />
      <label :for="idOfInput" style="font-size: 0.8rem;">{{ label }}</label>
    </div>
    <div v-if="!aligned" class="left-aligned" :style="{width: `${widthOfRow}rem`}">
      <label :for="idOfInput" :style="{ marginLeft: -c + 'rem'}">{{ label }}</label>
      <input
        :min="minChar"
        :max="maxChar"
        :disabled="read || notEditable"
        v-model.lazy="text"
        :value="value"
        :type="type"
        :id="idOfInput"
        :name="idOfInput"
        @input="handleInput($event.target.value)"
        :style="{
          marginLeft: `${c}rem`,
          maxWidth: `${widthOfInput}rem`,
          'background-color': `${colored}`,
        }"
      />
    </div>
  </div>
</template>

<script>
import uid from "uuid/v4";
export default {
  name: "SafaInput",
  data() {
    return {
      read: null,
      notEditable: null,
      aligned: null,
      idOfInput: null,
      text: "",
      values: true
    };
  },
  props: {
    align: {
      type: String,
      required: true,
      default: "right",
      validator: v => ["right", "left"].includes(v)
    },
    minChar: {
      type: Number
    },
    m: {
      // read write
      type: String,
      validator: v => ["r", "e", "ne"].includes(v),
      default: "e"
    },
    type: {
      type: String,
      default: "text",
      required: true
    },
    value: { type: String, default: "" },
    label: { default: "", type: String },
    placeholder: {
      type: String,
      default: ""
    },
    maxChar: {
      type: Number
    },
    // rahnameee
    helper: { type: String, default: "شما اینجایی" },
    errorlabel: { type: String, default: "تصحیح شود" },
    padding: {
      type: Number,
      default: 7
    },
    widthOfRow: {
      type: String
    },
    widthOfInput: {
      type: String
    },
    colored: {
      type: String,
      default: "amber-1"
    },
    c: {
      // label
      type: String,
      required: true,
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
  computed: {
    objectFromProp() {
      return {
        marginLeft: `${this.c}rem`
      };
    }
  },
  created() {
    this.$set(this, "text", this.value);
    const generatedId = "Safa" + "_" + uid();
    this.$set(this, "idOfInput", generatedId);
    this.align === "right"
      ? this.$set(this, "aligned", true)
      : this.$set(this, "aligned", false);
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
    reset() {
      this.$refs.input.resetValidation();
    },

    handleInput(val) {
      const { minChar, maxChar } = this;
      if (val.length < minChar && val.length > maxChar) {
        return;
      }
      this.$emit("inputer", val);
    }
  }
};
</script>

<style lang='scss'>
@import url("http://cdn.font-store.ir/behdad.css");
div {
  .aligned {
    font-family: "behdad", "Courier New", Courier, monospace;
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;

    input {
      border-radius: 4px;
      padding: 3px 4px;
      border: none;
      outline: none;
      direction: rtl;
      font-family: "behdad", "Courier New", Courier, monospace;

      &:invalid {
        border: 1px solid red;
      }
    }
    label {
    }
  }

  .left-aligned {
    font-family: "behdad", "Courier New", Courier, monospace;
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    // background-color: rgb(165, 157, 157);

    input {
      border-radius: 4px;
      border: none;
      background-color: rgb(224, 241, 255);
      outline: none;
      // background-color: red;
    }
    label {
    }
  }
}
</style>
