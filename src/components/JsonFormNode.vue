<template>
  <div>
    <label v-if="propName" :for="`${id}:${propName}`">
      {{ labels[propName] != null ? labels[propName] : propName }}
    </label>
    <input
      :id="`${id}:${propName}`"
      v-if="isScalar(value)"
      :type="inputType(value)"
      :placeholder="value"
      v-model="model"
      @change="bubble()"
    />
    <JsonFormInput
      class="pad"
      :id="`${id}:${propName}`"
      v-else-if="isObject(value)"
      :template="value"
      :labels="labels"
      @value-changed="(newValue) => registerChange(propName, newValue)"
    ></JsonFormInput>
    <ul v-else :id="`${id}:${propName}`">
      <li v-for="index in arrayItemsLength" :key="index">
        <JsonFormNode
          :value="value[0]"
          :labels="labels"
          @value-changed="
            (newValue) => registerArrayItemChange(index - 1, newValue)
          "
        ></JsonFormNode>
      </li>
    </ul>
  </div>
</template>

<script>
  import JsonFormInput from "@/components/JsonFormInput";

  let uid = 0;

  export default {
    name: "JsonFormNode",
    components: {
      JsonFormInput: () => Promise.resolve(JsonFormInput),
    },
    props: {
      propName: {
        type: String,
      },
      value: {
        required: true,
      },
      labels: {
        type: Object,
      },
    },
    data() {
      return {
        model: undefined,
        arrayItemsLength: 1,
        id: uid,
      };
    },
    beforeCreate() {
      uid++;
    },
    methods: {
      registerArrayItemChange(index, newValue) {
        if (!this.model) {
          this.model = [];
        }
        this.model[index] = newValue;
        this.bubble();
      },
      registerChange(propName, newValue) {
        if (!this.model) {
          this.model = {};
        }
        if (propName) {
          this.model[propName] = newValue;
        }
        this.model = newValue;
        this.bubble();
      },
      bubble() {
        this.$emit("value-changed", this.model);
      },
      inputType(value) {
        if (this.isScalar(value)) {
          return this.isString(value) ? "text" : "number";
        }
        return "object";
      },
      isScalar(value) {
        return this.isString(value) || this.isNumber(value);
      },
      isString(value) {
        return typeof value === "string";
      },
      isNumber(value) {
        return typeof value === "number";
      },
      isArray(value) {
        return Array.isArray(value);
      },
      isObject(value) {
        return typeof value === "object" && !this.isArray(value);
      },
    },
  };
</script>

<style scoped>
  .pad {
    padding-left: 2em;
    border-left: 2px solid coral;
  }
  ul {
    /*style reset*/
    list-style: none;
    padding: 0;
  }
</style>
