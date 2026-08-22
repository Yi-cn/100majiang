<script setup>
import { ref, computed, onBeforeUnmount } from 'vue'

const sentenceTypes = [
  {
    id: 'type1',
    label: '行动惩罚类',
    sentences: [
      "1. 手里摸到五万，立刻原地 3个深蹲，不做不能出牌",
      "2. 打出九条，起身绕麻将桌走一圈再坐下",
      "3. 听牌之后，全程只能用左手拿牌",
      "4. 每碰一次牌，做 4个开合跳",
      "5. 开杠瞬间，大声喊“杠上开花好运来”，忘喊深蹲 2个",
      "6. 摸到西风，站起来伸展手臂 10 秒",
      "7. 打出一筒，对家需要高抬腿 5次",
      "8. 选择留住三条，先完成 2个深蹲，打出则无需惩罚",
      "9. 只要有人自摸，你喝一小口水",
      "10. 打出八万，原地踮脚 10 下",
      "11. 摸到北风，不能立刻出牌，停顿 5秒钟",
      "12. 吃牌之后，深呼吸 3次才能继续摸牌",
      "13. 留住九万必须深蹲 3次，直接打出无事",
      "14. 打出七条，轻轻敲桌面三下",
      "15. 本局打算做小七对，每次摸牌前拍手两下",
      "16. 摸到红中，站起来转一圈",
      "17. 打出四条，下家深蹲 2个",
      "18. 宣布胡牌前，先说一句大吉大利",
      "19. 流局之后，所有人共同深呼吸一次",
      "20. 摸到发财，摸牌动作放慢一倍",
      "21. 打出南风，不能立刻摸下一张牌",
      "22. 每抓一张字牌，微微下蹲一次",
      "23. 打出六筒，左右扭动肩膀 5次",
      "24. 打算做清一色，摸到异色牌深蹲 2个",
      "25. 摸到白板，沉默三秒再操作",
      "26. 打出二万，和上家对视一秒",
      "27. 碰筒子牌，做 3次扩胸运动",
      "28. 打出幺鸡，模仿小鸡叫一声",
      "29. 摸到八条，原地小跳两下",
      "30. 听牌后打出的第一张牌，需要缓慢推出",
      "31. 摸到三万，必须原地转身 180 度再出牌",
      "32. 你打出四万时，下一位玩家必须做 1 次俯卧撑",
      "33. 摸到五筒，双手举过头顶保持 3 秒",
      "34. 打出一条时，必须先说出一句“真稳”再摸牌",
      "35. 听牌期间，所有动作都必须慢一拍，不能急躁"
    ]
  },
  {
    id: 'type2',
    label: '互动说话类',
    sentences: [
      "31. 手牌有 3、6、9万，每摸一张牌，大声报出牌名",
      "32. 持有二筒，每次出牌前，说一句“这牌绝了”",
      "33. 集齐 1、4、7筒，打牌时每轮说一句吉祥话",
      "34. 摸到幺鸡，轮到你时，必须说“幺鸡驾到，通通闪开”",
      "35. 有四张条牌，别人胡牌时，大喊“恭喜胡牌”",
      "36. 集齐 2、5、8筒，摸牌时大声喊“又来好牌”",
      "37. 手里有东风，出牌时说“东风吹，好运来”",
      "38. 手牌三张六万，每打一张牌，吐槽一句牌运",
      "39. 持有三筒，别人碰牌时，说“碰得好，下把我胡”",
      "40. 集齐 1、5、9万，全程边打边说“这把必胡”",
      "41. 摸到发财，出牌时说“发财啦，大家沾沾喜气”",
      "42. 手牌四张万子，每轮和下家聊一句天",
      "43. 手里有七筒，自摸时大喊“自摸啦，给钱给钱”",
      "44. 集齐 3、6、9条，摸牌时大声搓牌并喊“好牌好牌”",
      "45. 持有北风，等待别人出牌时，和对家闲聊",
      "46. 手牌一对五筒，每打一张牌，说一句“加油胡牌”",
      "47. 摸到九条，本轮每说一句话，都带“九条”二",
      "48. 集齐 2、5、8条，交流时大声说话，活跃气氛",
      "49. 手里三张一筒，每摸一张牌，碎碎念“一筒一筒”",
      "50. 有四张筒牌，别人开杠时，大喊“杠得漂亮”",
      "51. 手牌有二万，每次出牌前，说“二万出击，所向披靡”",
      "52. 持有四条，每轮和上家互动一句",
      "53. 集齐 1、2、3万，打牌时全程唱歌",
      "54. 摸到红中，出牌时说“红中在手，天下我有”",
      "55. 有三张条子，别人吃牌时，说“吃得妙，下把我要”",
      "56. 集齐 7、8、9筒，每摸一张牌，喊一句“胡牌在即”",
      "57. 手里有南风，出牌时说“南风送福，好运常驻”",
      "58. 手牌四张筒子，每打一张牌，和下家开玩笑",
      "59. 持有五万，自摸时说“五万自摸，快乐多多”",
      "60. 集齐 4、5、6条，全程大声说话，不许小声",
      "61. 打到九万时，必须喊一句“九万，别慌，我稳”",
      "62. 轮到你出牌时，必须和桌前任意一人说一句“发牌吧”",
      "63. 每次摸到字牌，都要先表演一个“好运来”的动作并喊口号",
      "64. 你碰牌之后，立刻对上家说一句“这回我看得准”",
      "65. 本局若你赢牌，必须高喊“我来开香槟！”"
    ]
  },
  {
    id: 'type3',
    label: '手牌规则类',
    sentences: [
      "61. 胡牌硬性条件：牌组包含自己手机号任意数字 + 一对将",
      "62. 手牌同时有一万九万，本局优先保留顺子，不能随便拆",
      "63. 手里集齐东南风，允许和对家互换一张手牌",
      "64. 拥有三张发财，可以偷看牌墙最末尾一张牌",
      "65. 手牌出现四张相同牌，点炮需要支付一半分数",
      "66. 开局手里两对字牌，可以重新置换两张手牌",
      "67. 手牌 1、4、7万齐全，不能轻易拆掉搭子",
      "68. 同时持有白板红中，听牌后别人点炮分数减半",
      "69. 手牌三条幺鸡，允许跳过一次吃牌机会",
      "70. 集齐万、条、筒各三张，自摸收益翻倍",
      "71. 手里一对西风，不可以率先打出字牌",
      "72. 手牌五张筒子，别人打出筒子优先考虑碰",
      "73. 集齐春夏秋冬任意两张，开局多摸一张牌",
      "74. 手牌四张条子，禁止强行做对对胡",
      "75. 同时拥有八万二万，优先组成顺子",
      "76. 手里三张九筒，听牌之后不能更换听口",
      "77. 集齐梅兰竹菊，本局不吃不碰额外加分",
      "78. 手牌一对发财一对白板，可互换一张手牌",
      "79. 拥有 1、3、5万，保留嵌张搭子",
      "80. 手里四张万子，点炮风险承担减半",
      "81. 手牌出现两组相连数字，优先保留顺子而不是拆散",
      "82. 持有三张同数字牌时，摸到同花色相邻牌可暂时不出",
      "83. 绝对不能在一轮内打出两张同一花色字牌",
      "84. 手牌中有两个对子时，若摸到同花色中间牌立即保留",
      "85. 若你有一组顺子和一对将，可直接选择不拆顺子"
    ]
  },
  {
    id: 'type4',
    label: '互动博弈类',
    sentences: [
      "81. 摸到五条：随机指定一名玩家，两人互换一张手牌，有人碰牌则跳过",
      "82. 打出七条：询问下家一个小问题，对方不回答不能继续摸牌",
      "83. 自摸胡牌：其余三人一同喝一口水",
      "84. 你点炮之后，可以和赢家石头剪刀布，赢了减半支付",
      "85. 有人杠牌，其余三名玩家集体深蹲 3个",
      "86. 流局全体齐喊：下一把必定胡牌",
      "87. 碰牌一次，和对家一同喝水",
      "88. 听牌之后全程站立，直到本局结束",
      "89. 开局可以拿出一张手牌，和牌墙最后一张互换",
      "90. 别人出牌阶段，你全身不能乱动，轮到摸牌解除",
      "91. 手里抓到五万，指定一人做 5次扩胸运动",
      "92. 完成一次吃牌，与上家互相点头示意",
      "93. 手牌无法凑搭子时，可以请求对手一次不强制提示",
      "94. 率先听牌的人，本轮拥有优先杠牌权利",
      "95. 连续三手没摸到有用牌，允许跳过一次出牌等待",
      "96. 打出字牌时，可以和任意对手对视三秒",
      "97. 手里缺少将牌，摸到对子优先留住",
      "98. 对手胡牌，你可以选择做深蹲抵消一半失分",
      "99. 本局不吃不碰不杠，如果成功胡牌积分翻倍",
      "100. 摸到任意字牌，可以强制和下家交换一张手牌",
      "101. 若有人放炮，所有人必须和他对视 2 秒再继续",
      "102. 每轮第一次摸牌时，必须先问一下上家“这把稳不稳”",
      "103. 打出 8 条时，指定一人选择是否喝水，拒绝则加一轮惩罚",
      "104. 当有人听牌时，所有人默认不许大声讲话直到本轮结束",
      "105. 自摸后，所有人一起说一句“好运守住了”"
    ]
  }
]

const selectedTypes = ref({
  type1: true,
  type2: true,
  type3: true,
  type4: true
})

const currentSentence = ref('规则随机抽取中……')
const isRunning = ref(false)
const intervalMs = ref(100)
let intervalId = null

const activeSentences = computed(() => {
  return sentenceTypes
    .filter((type) => selectedTypes.value[type.id])
    .flatMap((type) => type.sentences)
})

const clearIntervalTimer = () => {
  if (intervalId) {
    clearInterval(intervalId)
    intervalId = null
  }
}

const restartInterval = () => {
  clearIntervalTimer()

  if (!isRunning.value) return

  intervalId = setInterval(() => {
    pickRandomSentence()
  }, intervalMs.value)
}

const pickRandomSentence = () => {
  const pool = activeSentences.value

  if (!pool.length) {
    currentSentence.value = '规则随机抽取中……'
    return
  }

  const randomIndex = Math.floor(Math.random() * pool.length)
  currentSentence.value = pool[randomIndex]
}

const startDrawing = () => {
  if (isRunning.value || !activeSentences.value.length) return

  isRunning.value = true
  pickRandomSentence()
  restartInterval()
}

const stopDrawing = () => {
  isRunning.value = false
  clearIntervalTimer()
}

const updateSelection = () => {
  if (!activeSentences.value.length) {
    stopDrawing()
    currentSentence.value = '规则随机抽取中……'
    return
  }

  if (!isRunning.value) {
    currentSentence.value = activeSentences.value[0]
  }
}

const handleIntervalChange = () => {
  if (isRunning.value) {
    restartInterval()
  }
}

onBeforeUnmount(() => {
  stopDrawing()
})
</script>

<template>
  <main class="app-shell">
    <section class="sentence-card">
      <p class="eyebrow">麻将海克斯</p>
      <h1>本局规则</h1>

      <div class="type-filters">
        <label v-for="type in sentenceTypes" :key="type.id" class="filter-item">
          <input v-model="selectedTypes[type.id]" type="checkbox" @change="updateSelection" />
          <span>{{ type.label }}</span>
        </label>
      </div>

      <div class="interval-control">
        <label for="interval-range">抽取间隔：{{ intervalMs }}ms</label>
        <input
          id="interval-range"
          v-model.number="intervalMs"
          type="range"
          min="30"
          max="2000"
          step="25"
          @input="handleIntervalChange"
        />
      </div>

      <blockquote>{{ currentSentence }}</blockquote>

      <div class="controls">
        <button class="start-button" @click="startDrawing" :disabled="isRunning || !activeSentences.length">
          Start
        </button>
        <button class="stop-button" @click="stopDrawing" :disabled="!isRunning">
          Stop
        </button>
      </div>
    </section>
  </main>
</template>
