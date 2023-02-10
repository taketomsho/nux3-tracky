<template>
    <v-app>
  <!-- ヘッダー部分 -->
    <Header />
    
    <!-- メインコンテンツ -->
    <v-main>
    <v-container class="text-center">
      <h1 class="my-5">記事分析AI Tracky</h1>
      <p class="title_below mb-5">たった1分であなたの記事の改善点が分かる「AI SEOツール」！</p>
    </v-container>
    
    <v-container>
          <v-row>
            <v-col cols="12" md="6">
                <v-container>
                  <v-form ref="form" v-model="valid" lazy-validation>
                  <p class="mb-3 text-center">あなたの記事の順位を上げるために必要なポイントをAIが洗い出します</p>
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
                  <p class="mt-3 text-center">※少々分析にお時間がかかる場合がございます。</p>
                </v-form>
                </v-container>
            </v-col>
            <v-col cols="12" md="6">
              <v-container class="justify-center">
                <img src="~/assets/image/tracky_service_demo.png" width="300">
              </v-container>
            </v-col>
          </v-row>
      </v-container>
          
    <v-container>
      <circular v-if="status === 'processing'" />
      <dashboard v-bind:calcurated="result" v-if="status === 'completed'" />

    </v-container>
  </v-main>
  <Footer />
  </v-app>
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

<style>

/* H1の装飾 */
 h1 {
  position: relative;
  padding: 1rem 2rem calc(1rem + 10px);
  background: #F7DDBA;
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
.title_below {
  position: relative;
  display: inline-block;
  padding: 0 55px;
}

.title_below:before, .title_below:after {
  content: '';
  position: absolute;
  top: 50%;
  display: inline-block;
  width: 45px;
  height: 1px;
  background-color: black;
}

.title_below:before {
  left:0;
}
.title_below:after {
  right: 0;
}
.title_below {
  font-size: 25px;
}

/* H2の装飾 */
/* h2 {
  position: relative;
  padding: 5px 26px 5px 42px;
  background: #fff0d9;
  font-size: 20px;
  color: #2d2d2d;
  margin-left: 33px;
  margin-right: 33px;
  line-height: 1.3;
  border-bottom: solid 3px orange;
  z-index:-2;
}

h2:before {
  position: absolute;
  content: '';
  left: -2px;
  top: -2px;
  width: 0;
  height: 0;
  border: none;
  border-left: solid 40px white;
  border-bottom: solid 79px transparent;
  z-index: -1;
}

h2:after {
  position: absolute;
  content: '';
  right: -3px;
  top: -7px;
  width: 0;
  height: 0;
  border: none;
  border-left: solid 40px transparent;
  border-bottom: solid 79px white;
  z-index: -1;
} */

h2 {
  position: relative;
  color: #333;
  display: inline-block;
  margin: 47px 0;
  text-shadow: 0 0 2px white;
}
h2:before {
  content: "";
  position: absolute;
  background: #ffd69d;
  width: 70px;
  height: 70px;
  border-radius: 50%;
  top: 50%;
  left: 50%;
  -webkit-transform: translate(-50%,-50%);
  transform: translate(-50%,-50%);
  z-index: -1;
}

/* ------------------------------ */
/* ヘッダー */
/* ------------------------------ */

.header02 {
  padding: 16px 24px;
  background-color: #fafafa;
}

.header02-list {
  display: flex;
  justify-content: center;
  gap: 32px;
  font-size: 16px;
  font-weight: bold;
  margin-top: 24px;
}
ul {
  list-style: none;
}

</style>