<template>
  <div>
    <h1>Tracky</h1>
  </div>
  <v-form ref="form" v-model="valid" lazy-validation>
    <v-container>
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
      <v-btn variant="tonal" @click="analyze"> 分析開始 </v-btn>
    </v-container>
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
