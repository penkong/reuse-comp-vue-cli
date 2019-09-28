<template>
  <div>
    <div v-if="aligned" class="safa-combo-box row" :style="{minHeigh: height, minWidth: width }">
      <div>
        <Vselect
          :placeholder="placeholder"
          :options="iterator(this.items)"
          v-model="selected"
          @input="setSelected"
          :inputId="idOfInput"
          :disabled="read || notEditable"
          class="style-chooser"
          :style="{minWidth: width }"
        >
          <template #search="{attributes, events}">
            <input class="vs__search" :required="!selected" v-bind="attributes" v-on="events" />
          </template>
        </Vselect>
      </div>
      <div class="input-container" v-if="allowInput">
        <input
          :readonly="read"
          :disable="notEditable"
          :id="idOfInput"
          type="number"
          :style="{
            height: '1.5rem',
            width: '3rem',
            padding: '1px 3px',
            border: 'none',
            borderRadius: '3px',
            ouline: 'none',
            backgroundColor: '#d4ecff',
          }"
          v-model="inputNum"
          @change="findTitle"
        />
      </div>
      <label :for="idOfInput" class="label-container label" :style="{right: -c + 'px'}">{{ label }}</label>
    </div>
    <div v-else class="safa-combo-box row" :style="{minHeigh: height, minWidth: width }">
      <label
        :for="idOfInput"
        class="label-container left-label"
        :style="{ left: -c + 'rem'}"
      >{{ label }}</label>
      <div class="input-container">
        <input
          :readonly="read"
          :disable="notEditable"
          :id="idOfInput"
          type="number"
          :style="{
            height:'1rem'
          }"
          @input="handleInput($event)"
          v-model="inputNum"
        />
      </div>
      <div>
        <Vselect
          :placeholder="placeholder"
          :options="items"
          :value="selected"
          @input="setSelected"
          :inputId="idOfInput"
          :disabled="read || notEditable"
          class="style-chooser"
          :style="{minWidth: width }"
        >
          <template #search="{attributes, events}">
            <input class="vs__search" :required="!selected" v-bind="attributes" v-on="events" />
          </template>
        </Vselect>
      </div>
    </div>
  </div>
</template>


<script>
import uid from "uuid";
import Vselect from "vue-select";
import "vue-select/dist/vue-select.css";
export default {
  name: "SafaComboBox",
  data() {
    return {
      aligned: null,
      read: null,
      notEditable: null,
      idOfInput: null,
      inputNum: "",
      selected: "",
      lastId: null,
      error: false,
      itemsFromData: null,
      optionz: null
    };
  },
  components: {
    Vselect
  },
  props: {
    allowInput: {
      type: Boolean,
      default: true
    },
    m: {
      type: String,
      validator: v => ["r", "e", "ne"].includes(v),
      default: function() {
        return "r";
      }
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
      default: function() {
        return "4";
      }
    },
    align: {
      type: String,
      default: "right",
      validator: v => ["right", "left"].includes(v)
    },
    height: {
      type: String,
      default: "200px",
      required: true
    },
    width: {
      type: String,
      default: "200px",
      required: true
    },
    value: {
      type: String
    },
    label: {
      type: String,
      default: function() {
        return "none";
      }
    },
    dense: {
      type: Boolean,
      default: false
    },
    placeholder: {
      type: String
    },
    helper: {
      type: String,
      default: "تصحیح شود"
    },
    items: {
      type: Array,
      default: function() {
        return [];
      }
    },
    // come from db => go to serveice
    DtoName: {
      type: String,
      default: function() {
        return "";
      }
    },
    // data provider =? go to serveice sql mongo post gress
    Domain: {
      type: String,
      default: function() {
        return "";
      }
    },
    HideValue: {
      type: Boolean,
      default: function() {
        return false;
      }
    }
  },
  created() {
    if (this.items) {
      this.$set(this, "lastId", this.items.length);
    }
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
  computed: {
    cssProps() {
      if (this.align === "right") {
        return {
          fontSize: "0.80rem"
        };
      } else {
        return {
          float: "left",
          fontSize: "0.80rem"
        };
      }
    }
  },
  methods: {
    iterator() {
      let newArr = [];
      let keysMap = {
        title: "label",
        id: "code"
      };
      function renameKeys(obj, newKeys) {
        const keyValues = Object.keys(obj).map(key => {
          const newKey = newKeys[key] || key;
          return { [newKey]: obj[key] };
        });
        return Object.assign({}, ...keyValues);
      }
      this.items.forEach(el => {
        newArr.push(renameKeys(el, keysMap));
      });
      this.$set(this, "optionz", newArr);
      return newArr;
    },
    findTitle() {
      if (this.inputNum > this.lastId) this.inputNum = this.lastId;
      if (this.inputNum < 0) {
        this.selected = "";
        this.inputNum = 0;
        return null;
      }
      this.selected = this.iterator(this.items).find(el => {
        return el.code === this.inputNum;
      });
    },
    setSelected(val) {
      if (val) {
        if (!val.label) return null;
        this.selected = val.label;
        this.inputNum = val.code;
        this.$emit("selectedVal", val);
      } else {
        this.selected = null;
        this.inputNum = null;
      }
    }
  }
};
</script>
<style lang="scss" >
@import url("http://cdn.font-store.ir/behdad.css");
.safa-combo-box {
  font-family: "behdad", "Courier New", Courier, monospace;
  padding: 0;
  margin: 0;
  width: auto;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  font-size: 0.8rem;

  .style-chooser .vs__search::placeholder,
  .style-chooser .vs__dropdown-toggle,
  .style-chooser .vs__dropdown-menu {
    background: "lime-1";
    border: none;
    color: #0070cc;
    direction: rtl;
    text-align: right;
  }
  .style-chooser .vs__dropdown-toggle {
    margin: 0 4px;
  }
  .style-chooser .vs__clear,
  .style-chooser .vs__open-indicator {
    fill: #0070cc;
    margin-left: 6px;
  }
  .input-container {
    position: relative;
    margin: 0;
    padding: 0;
    margin-left: 5px;
    width: 70px;
    font-family: "Courier New", Courier, monospace;
  }
  .label-container {
    position: relative;
    & .label {
      position: absolute;
      padding: 0 4px;
      bottom: -2.4rem;
    }
    & .left-label {
      position: absolute;
      padding: 0 4px;
      bottom: -2.4rem;
    }
  }
}
</style>
