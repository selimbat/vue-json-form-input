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
    />
    <JsonFormInput
      class="pad"
      :id="`${id}:${propName}`"
      v-else-if="isObject(value)"
      :template="value"
      :labels="labels"
    ></JsonFormInput>
      <li v-for="(item, index) in value" :key="index">
        <JsonFormNode :value="item" :labels="labels"></JsonFormNode>
    <ul v-else :id="`${id}:${propName}`">
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
        id: uid,
      };
    },
    beforeCreate() {
      uid++;
    },
    methods: {
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
