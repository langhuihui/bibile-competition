<template>
    <n-config-provider :theme-overrides="themeOverrides">
      <n-layout>
        <n-layout-header>
          <n-space><n-input-number v-for="n of group" :default-value="0"></n-input-number></n-space>
        </n-layout-header>
        <n-layout-content style="width:80vw;height:70vh">
          <n-card>
            <n-space vertical>
              <div class="question">{{ currentQuestion.question }}</div>
              <n-space>
                <n-radio-group v-model:value="selectedOption">
                  <n-radio-button class="option" size="large" :label="option" v-for="(option,optionIndex ) in currentQuestion.answers" :key="option" :value="optionIndex"></n-radio-button>
                </n-radio-group>
              </n-space>
              <n-space justify="end">
                <n-button @click="submitAnswer" size="large">提交</n-button>
              </n-space>
             
            </n-space>
            <n-alert v-if="showResult" :type="isCorrect ? 'success' : 'error'" title="结果">
              {{ isCorrect ? '回答正确！' : '回答错误！正确答案：'+currentQuestion.answers[currentQuestion.correctAnswer] }}
            </n-alert>
            <n-button v-if="showNextButton" @click="nextQuestion">下一题</n-button>
          </n-card>
        </n-layout-content>
        <n-layout-footer>
          <n-slider v-model:value="currentQuestionIndex" />
        </n-layout-footer>
      </n-layout>
    </n-config-provider>
  </template>
  
  <script setup>
  import { computed, ref } from 'vue';
  import {NSlider,NInputNumber, NConfigProvider, NLayout, NLayoutContent, NLayoutHeader, NLayoutSider, NLayoutFooter, NMenu, NButton, NRadioGroup, NRadio, NCard, NAlert, NSpace, NRadioButton } from 'naive-ui';
  import questions from './question.json'
 
  const group = ref(7)
  const currentQuestionIndex = ref(0);
  const currentQuestion = computed(()=>questions[currentQuestionIndex.value])
  const selectedOption = ref(null);
  const showResult = ref(false);
  const isCorrect = ref(false);
  const showNextButton = ref(false);
  
  const submitAnswer = () => {
    showResult.value = true;
    isCorrect.value = selectedOption.value === currentQuestion.value.correctAnswer;
    showNextButton.value = true;
  };
  
  const nextQuestion = () => {
    currentQuestionIndex.value++;
    selectedOption.value = null;
    showResult.value = false;
    showNextButton.value = false;
  };
  
  const themeOverrides = {
    common: {
      // 自定义主题颜色
      primaryColor: '#1890ff',
      primaryColorHover: '#40a9ff',
      primaryColorPressed: '#096dd9',
      primaryColorSuppl: '#40a9ff'
    }
  };
  </script>
  <style>
.question {
  font-size: 2rem;
}
.option {
  /* font-size: 2rem; */
  /* min-height: 100px; */
  /* min-width: 300px; */
  /* margin: 10px; */
  /* line-height: 4rem; */
}
</style>