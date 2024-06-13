<template>
  <n-config-provider :theme-overrides="themeOverrides">
    <n-layout>
      <n-layout-header>知识竞赛</n-layout-header>
      <n-layout-content>
        <n-space vertical>
          <n-radio-group v-model:value="selectedGroup">
            <n-radio v-for="(group, index) in questionGroups" :key="index" :label="index">{{ group.name }}</n-radio>
          </n-radio-group>
          <n-space vertical>
            <n-card v-for="(question, index) in selectedQuestions" :key="index" :style="{ opacity: answeredQuestions[index] ? 0.5 : 1 }">
              <template #header>{{ question.question }}</template>
              <n-radio-group v-model:value="answers[index]">
                <n-radio v-for="(answer, answerIndex) in question.answers" :key="answerIndex" :label="answerIndex">{{ answer }}</n-radio>
              </n-radio-group>
            </n-card>
          </n-space>
          <n-button @click="submitAnswers">提交答案</n-button>
          <n-alert v-if="showResult" :type="result.correct === result.total ? 'success' : 'error'" :closable="false" style="width: 100%">
            你答对了 {{ result.correct }} 题，共 {{ result.total }} 题。
          </n-alert>
        </n-space>
      </n-layout-content>
    </n-layout>
  </n-config-provider>
</template>

<script setup>
import { ref, computed } from 'vue';
import { NConfigProvider, NLayout, NLayoutHeader, NLayoutContent, NSpace, NRadioGroup, NRadio, NCard, NButton, NAlert } from 'naive-ui';

const themeOverrides = {
  common: {
    primaryColor: '#18a058'
  }
};

const questions = [
  // ... 你的题目数据
];

const questionGroups = [
  // ... 你的题目分组数据
];

const selectedGroup = ref(null);
const answers = ref({});
const answeredQuestions = ref({});

const selectedQuestions = computed(() => {
  if (selectedGroup.value !== null) {
    return questionGroups[selectedGroup.value].questions.map(index => questions[index]);
  }
  return [];
});

const submitAnswers = () => {
  let correct = 0;
  selectedQuestions.value.forEach((question, index) => {
    if (answers.value[index] === question.correctAnswer) {
      correct++;
      answeredQuestions.value[index] = true;
    }
  });
  result.value = { correct, total: selectedQuestions.value.length };
  showResult.value = true;
};

const result = ref({ correct: 0, total: 0 });
const showResult = ref(false);
</script>

<style scoped>
.n-radio-group {
  margin-bottom: 16px;
}
</style>