<template>
  <div class="cmake-element">
    <div class="field">
      <div class="control">
        <label :for="el.id" class="label">{{ el.title }} </label>
      </div>
      <ul v-if="el.type === 'array'">
        <li v-for="v in el.value" :key="v" class="field is-grouped">
          <p class="label">{{ v }}</p>
          <div class="icon" @click="removeFromArray(v)">
            <iconify-icon icon="close" />
          </div>
        </li>
      </ul>
    </div>
    <div v-if="el.type === 'array'" class="field is-grouped">
      <div class="control">
        <input
          type="text is-small"
          v-model="elementValueToPush"
          class="input"
          @keyup.enter="addToArray"
        />
      </div>
      <div class="control">
        <div class="icon" @click="addToArray">
          <iconify-icon icon="add" />
        </div>
      </div>
    </div>
    <div v-else class="field">
      <div class="control">
        <input type="text is-small" v-model="el.value" class="input" />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import { State } from "vuex-class";
import { CmakeListsElement } from "../../../cmake/cmakeListsElement";

@Component
export default class CMakeListElement extends Vue {
  @Prop() public el: CmakeListsElement;
  @State("textDictionary") private storeTextDictionary;
  private valueToPush: string;

  get elementValueToPush() {
    return this.valueToPush;
  }
  set elementValueToPush(newVal: string) {
    this.valueToPush = newVal;
  }

  get saveText() {
    return this.storeTextDictionary.save;
  }
  get cancelText() {
    return this.storeTextDictionary.discard;
  }

  public removeFromArray(value) {
    const index = this.el.value.indexOf(value);
    this.el.value.splice(index, 1);
  }

  public addToArray() {
    this.el.value.push(this.valueToPush);
    this.valueToPush = "";
  }
}
</script>

<style scoped>
.icon:hover {
  background-color: var(--vscode-button-background);
}
.is-grouped {
  align-items: center;
}
li.is-grouped .icon {
  margin-bottom: 0.5em;
}
</style>
