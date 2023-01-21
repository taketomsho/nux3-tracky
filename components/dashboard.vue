<template>
  <div>
    <v-card color="basil">
      <v-toolbar color="primary">
        <v-app-bar-nav-icon></v-app-bar-nav-icon>

        <v-toolbar-title>実行結果</v-toolbar-title>

        <v-spacer></v-spacer>

        <v-btn icon>
          <v-icon>mdi-magnify</v-icon>
        </v-btn>

        <v-btn icon>
          <v-icon>mdi-dots-vertical</v-icon>
        </v-btn>

        <template v-slot:extension>
          <v-tabs v-model="tab" align-tabs="title">
            <v-tab v-for="item in items" :key="item" :value="item">
              {{ item }}
            </v-tab>
          </v-tabs>
        </template>
      </v-toolbar>

      <v-window v-model="tab">
        <v-window-item v-for="item in items" :key="item" :value="item">
          <v-card flat>
            <v-card-text v-text="text"></v-card-text>
          </v-card>
        </v-window-item>
      </v-window>
    </v-card>
  </div>
</template>

<script setup>
const tab = ref(null);
const items = [
  '総合評価',
  '文字数',
  'キーワード',
  '見出し',
  'リンク',
  '記事の新しさ',
];
const props = defineProps({
  text: String,
});
</script>
