<template>
  <div>
    
  </div>
  <div class="container text-center">
    <h1 class="my-5">記事分析AI Tracky</h1>
    <p class="title_blow mb-5">たった1分であなたの記事の改善点が分かる「AI SEOツール」！</p>
  </div>
  
  <v-form ref="form" v-model="valid" lazy-validation>
    <div class="d-flex justify-start">
    <v-container>
      <p class="my-3 text-center">あなたの記事の順位を上げるために必要なポイントをAIが洗い出します</p>
      <v-col>
      <v-text-field
        v-model="keyword"
        :rules="keywordRules"
        label="keyword"
        required
      ></v-text-field>
      <v-text-field
        v-model="url"
        :rules="urlRules"
        label="url"
        required
      ></v-text-field>
      <v-row class="justify-center mt-2">
      <v-btn variant="tonal" @click="analyze"> 分析開始 </v-btn>
      </v-row>
      </v-col>
      <p class="my-5 text-center">※少々分析にお時間がかかる場合がございます。</p>
    </v-container>
    <v-container>
      <img src="~/assets/image/tracky_service_demo.png">
    </v-container>
  </div>
  </v-form>
  <v-container>
    <circular v-if="status === 'processing'" />
    <dashboard v-bind:calcurated="result" v-if="status === 'completed'" />

  </v-container>
</template>

<script setup>

const keyword = ref('');
const url = ref('');
const valid = ref(false);
const keywordRules = [
  (v) => !!v || 'Name is required',
  (v) => v.length <= 1000 || 'Name must be less than 10 characters',
];

const urlRules = [
  (v) => !!v || 'E-mail is required',
  (v) => /.+.+/.test(v) || 'E-mail must be valid',
];

const result = ref(null);
const status = ref('ready');
async function analyze() {
  status.value = 'processing';
  const { data, pending, error, refresh } = await useLazyFetch(
    'https://scraping-euy6vi4i2a-an.a.run.app',
    {
      query: {
        keyword: keyword,
        url: url,
      },
    }
  );
  status.value = 'completed';
  result.value = data;
}
</script>

<style scoped>

/* タイトルの装飾 */
 h1 {
  position: relative;
  padding: 1rem 2rem calc(1rem + 10px);
  background: #fff100;
}

h1:before {
  position: absolute;
  top: -7px;
  left: -7px;
  width: 100%;
  height: 100%;
  content: '';
  border: 4px solid #000;
}

/* ここからタイトル下の文章の装飾 */
.title_blow {
  position: relative;
  display: inline-block;
  padding: 0 55px;
}

.title_blow:before, .title_blow:after {
  content: '';
  position: absolute;
  top: 50%;
  display: inline-block;
  width: 45px;
  height: 1px;
  background-color: black;
}

.title_blow:before {
  left:0;
}
.title_blow:after {
  right: 0;
}
.title_blow {
  font-size: 25px;
}

</style>