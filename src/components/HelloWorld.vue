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
  {
    question: "马太福音第一章主要讲述了耶稣的诞生。（判断）",
    answers: ["正确", "错误"],
    correctAnswer: 1
  },
  {
    question: "耶稣的母亲是玛利亚。（判断）",
    answers: ["正确", "错误"],
    correctAnswer: 1
  },
  {
    question: "马太福音第一章中只提到了约瑟。（判断）",
    answers: ["正确", "错误"],
    correctAnswer: 0
  },
  {
    question: "耶稣是大卫的后裔。（判断）",
    answers: ["正确", "错误"],
    correctAnswer: 1
  },
  {
    question: "马太福音第一章详细描述了耶稣诞生的具体日期。（判断）",
    answers: ["正确", "错误"],
    correctAnswer: 0
  },
  {
    question: "亚伯拉罕是耶稣的祖先。（判断）",
    answers: ["正确", "错误"],
    correctAnswer: 1
  },
  {
    question: "马太福音第一章记载的人物都是真实存在的历史人物。（判断）",
    answers: ["正确", "错误"],
    correctAnswer: 1
  },
  {
    question: "约瑟在梦中得到天使的指示。（判断）",
    answers: ["正确", "错误"],
    correctAnswer: 1
  },
  {
    question: "马太福音第一章中提到了很多其他的圣经人物。（判断）",
    answers: ["正确", "错误"],
    correctAnswer: 1
  },
  {
    question: "耶稣诞生的地点在耶路撒冷。（判断）",
    answers: ["正确", "错误"],
    correctAnswer: 0
  },
  {
    question: "马太福音第一章开始记载的是谁的谱系？",
    answers: ["耶稣", "约瑟", "玛利亚"],
    correctAnswer: 1
  },
  {
    question: "耶稣的母亲玛利亚是从哪里来的？",
    answers: ["耶路撒冷", "加利利的拿撒勒", "伯利恒"],
    correctAnswer: 2
  },
  {
    question: "约瑟最初得知玛利亚怀孕时打算怎么做？",
    answers: ["高兴", "告诉其他人", "暗暗地休了她"],
    correctAnswer: 3
  },
  {
    question: "天使向约瑟显现时说玛利亚所怀的是？",
    answers: ["圣子", "圣灵", "圣徒"],
    correctAnswer: 1
  },
  {
    question: "耶稣诞生后被放在？",
    answers: ["马槽里", "床上", "马棚里"],
    correctAnswer: 3
  },
  {
    question: "马太福音第一章提到约瑟是（  ）的子孙。",
    answers: ["亚伯拉罕", "大卫", "以撒"],
    correctAnswer: 2
  },
  {
    question: "以下人物中，不是马太福音第一章提到的是？",
    answers: ["亚伯拉罕", "以赛亚", "保罗"],
    correctAnswer: 3
  },
  {
    question: "约瑟给耶稣起的名字是根据（  ）。",
    answers: ["天使的指示", "自己的想法", "玛利亚的要求"],
    correctAnswer: 1
  },
  {
    question: "马太福音第一章记载了耶稣的家族谱系共有多少代？",
    answers: ["40", "42", "44"],
    correctAnswer: 2
  },
  {
    question: "玛利亚在身孕很重的时候，和约瑟去（  ）报名上册。",
    answers: ["伯利恒", "耶路撒冷", "加利利"],
    correctAnswer: 1
  }
];

const questionGroups = [
  {
    name: "第一章",
    questions:[0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19]
  },
  {
    name: "第二章"
  }
];

const selectedGroup = ref(0);
const answers = ref({});
const answeredQuestions = ref({});

const selectedQuestions = computed(() => questionGroups[selectedGroup.value].questions.map(index => questions[index]));

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