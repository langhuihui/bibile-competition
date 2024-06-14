<template>
    <n-config-provider :theme-overrides="themeOverrides">
      <n-layout>
        <n-layout-content>
          <n-card>
            <n-space vertical>
              <div class="question">{{ currentQuestion.question }}</div>
              <n-space>
                <n-radio-group v-model:value="selectedOption">
                  <n-radio :label="option" v-for="(option,optionIndex ) in currentQuestion.answers" :key="option" :value="optionIndex">
                    {{ option }}
                  </n-radio>
                </n-radio-group>
              </n-space>
              <n-space justify="end">
                <n-button @click="submitAnswer">提交</n-button>
              </n-space>
            </n-space>
            <n-alert v-if="showResult" :type="isCorrect ? 'success' : 'error'" title="结果">
              {{ isCorrect ? '回答正确！' : '回答错误！正确答案：'+currentQuestion.answers[currentQuestion.correctAnswer] }}
            </n-alert>
            <n-button v-if="showNextButton" @click="nextQuestion">下一题</n-button>
          </n-card>
        </n-layout-content>
      </n-layout>
    </n-config-provider>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  import { NConfigProvider, NLayout, NLayoutContent, NLayoutHeader, NLayoutSider, NLayoutFooter, NMenu, NButton, NRadioGroup, NRadio, NCard, NAlert, NSpace } from 'naive-ui';
  import { questions } from './questions'
 
  
  const currentQuestionIndex = ref(0);
  const currentQuestion = ref(questions[currentQuestionIndex.value]);
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
    currentQuestion.value = questions[currentQuestionIndex.value];
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
  font-size: xx-large;
}
</style>