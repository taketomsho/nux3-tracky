<script setup>
import { Radar } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale,LineElement,PointElement,RadialLinearScale  } from 'chart.js'

ChartJS.register(
  Title, 
  Tooltip, 
  Legend,
  LineElement,
  PointElement, 
  BarElement, 
  CategoryScale, 
  LinearScale,
  RadialLinearScale,
)


const tab = ref(null);
const advice = "がんばれ！"
const props = defineProps({
  calcurated: Object,
});

const calc = JSON.parse(props.calcurated.value)

const chartData = {
    //データ項目のラベル
    labels: ["文字数","見出し数","画像数","リンク数","キーワード数","更新性"],
    //データセット
    datasets: [
        {
            label: "評価",
            //背景色
            backgroundColor: "rgb(240,188,115, 0.5)",
            //枠線の色
            borderColor: "rgb(240,188,115)",
            //結合点の背景色
            pointBackgroundColor: "rgb(240,188,115)",
            //結合点の枠線の色
            pointBorderColor: "#fff",
            //結合点の背景色（ホバ時）
            pointHoverBackgroundColor: "#fff",
            //結合点の枠線の色（ホバー時）
            pointHoverBorderColor: "rgb(240,188,115)",
            //結合点より外でマウスホバーを認識する範囲（ピクセル単位）
            hitRadius: 5,
            //グラフのデータ
            data: calc.score.data
        }
    ]
}

const chartOptions = {
  // レスポンシブ指定
  responsive: true,
  scale: {
    ticks: {
      // 最小値の値を0指定
      beginAtZero:true,
      min: 0,
      // 最大値を指定
      max: 100,
    }
  }
}

</script>


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
            <v-tab :value="1">
              総合評価
            </v-tab>
            <v-tab :value="2">
              文字数
            </v-tab>
            <v-tab :value="3">
              画像
            </v-tab>
            <v-tab :value="4">
              見出し
            </v-tab>
            <v-tab :value="5">
              リンク
            </v-tab>
            <v-tab :value="6">
              記事の新しさ
            </v-tab>
          </v-tabs>
        </template>
      </v-toolbar>

      <v-window v-model="tab">
        <v-window-item :value="1">
          <v-card flat>
            <v-card-text v-text="advice"></v-card-text>

            <Radar
            id="my-chart-id"
            :options="chartOptions"
            :data="chartData"
            />
          </v-card>
        </v-window-item>

        <v-window-item :value="2">
          <v-card flat>
            <v-card-text v-text="advice"></v-card-text>

            <Boxplotchart
              v-bind:boxdata="calc.title.data"
            />
            <Boxplotchart
              v-bind:boxdata="calc.meta.data"
            />
            <Boxplotchart
              v-bind:boxdata="calc.body.data"
            />
          </v-card>
        </v-window-item>
        <v-window-item :value="3">
          <v-card flat>
            <v-card-text v-text="advice"></v-card-text>

            <Boxplotchart
              v-bind:boxdata="calc.img.data"
            />

          </v-card>
        </v-window-item>
        <v-window-item :value="4">
          <v-card flat>
            <v-card-text v-text="advice"></v-card-text>

            <Boxplotchart
              v-bind:boxdata="calc.h2.data"
            />
            <Boxplotchart
              v-bind:boxdata="calc.h3.data"
            />
            <Boxplotchart
              v-bind:boxdata="calc.h4.data"
            />
          </v-card>
        </v-window-item>
        <v-window-item :value="5">
          <v-card flat>
            <v-card-text v-text=""></v-card-text>

            <Boxplotchart
              v-bind:boxdata="calc.inner_link.data"
            />
            <Boxplotchart
              v-bind:boxdata="calc.outer_link.data"
            />
          </v-card>
        </v-window-item>
        <v-window-item :value="6">
          <v-card flat>
            <v-card-text v-text="advice"></v-card-text>

            <Boxplotchart
              v-bind:boxdata="calc.freshness.data"
            />
          </v-card>
        </v-window-item>
      </v-window>
    </v-card>
  </div>
</template>

