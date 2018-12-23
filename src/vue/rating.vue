<template>
    <div>
        <div :class="question.cssClasses.root">
            <label v-for="(item, index) in question.visibleRateValues" :key="item.value" :class="getCss(question, item)">
                <input type="radio" style="display: none;" :name="question.name" :id="question.name + index" :value="item.value" :disabled="question.isReadOnly" @change="change" v-bind:aria-label="item.locText.text"/>
                <span :class="question.cssClasses.itemText"><survey-string :locString="item.locText"/></span>
            </label>
        </div>
        <div :class="question.cssClasses.root"> 
            <template v-for="(item, index) in question.visibleRateValues" :class="getCss(question, item)">
                <span v-if="index === 0" :class="question.cssClasses.minText"><survey-string :locString="question.locMinRateDescription"/></span>
                <span v-else-if="index === question.visibleRateValues.length-1" :class="question.cssClasses.maxText"><survey-string :locString="question.locMaxRateDescription"/></span>
            </template>
        </div>
        <survey-other-choice v-show="question.hasOther" :class="question.cssClasses.other" :question="question"/>
    </div>
</template>

<script lang="ts">
import Vue from "vue";
import { Component, Prop } from "vue-property-decorator";
import { default as QuestionVue } from "./question";
import { QuestionRatingModel } from "../question_rating";

@Component
export class Rating extends QuestionVue<QuestionRatingModel> {
  getCss(question: QuestionRatingModel, item:any) {
    let css = question.cssClasses.item;
    if (question.value == item.value) {
      css = css + " " + question.cssClasses.selected;
    }
    return css;
  }
  change(e:any) {
    this.question.value = e.target.value;
  }
}
Vue.component("survey-rating", Rating);
    export default Rating;
</script>
