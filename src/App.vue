<template>
  <div class="app">
    <!-- 背景粒子 -->
    <div class="particles">
      <span v-for="i in 20" :key="i" class="particle" :style="particleStyle(i)">⚽</span>
    </div>

    <!-- 标题 -->
    <header class="title-area">
      <h1 class="main-title">🏆 足球AI模拟对战 🏆</h1>
      <p class="sub-title">FIFA WORLD CUP AI SIMULATION</p>
    </header>

    <!-- 对战双方 -->
    <div class="vs-area">
      <div class="team team-left" :class="{ 'team-win': showResult }">
        <div class="flag">🇨🇳</div>
        <div class="team-name">中国</div>
        <div class="team-rank">FIFA Ranking: 87</div>
      </div>

      <div class="vs-badge">
        <div class="vs-text">VS</div>
        <div class="vs-glow"></div>
      </div>

      <div class="team team-right" :class="{ 'team-lose': showResult }">
        <div class="flag">🇧🇷</div>
        <div class="team-name">巴西</div>
        <div class="team-rank">FIFA Ranking: 5</div>
      </div>
    </div>

    <!-- 开始按钮 -->
    <div class="action-area" v-if="!started">
      <button class="start-btn" @click="startSimulation">
        <span class="btn-icon">⚽</span>
        <span class="btn-text">开始AI模拟对战</span>
      </button>
      <p class="disclaimer">* 本模拟基于AI深度学习，结果绝对真实可靠</p>
    </div>

    <!-- 进度条 -->
    <div class="progress-area" v-if="started && !showResult">
      <div class="progress-container">
        <div class="progress-bar" :style="{ width: progress + '%' }">
          <div class="progress-glow"></div>
        </div>
        <div class="progress-text">{{ Math.floor(progress) }}%</div>
      </div>
      <div class="simulating-text">
        <span class="loading-dot">🔴</span> 模拟对战中...
        <span class="ai-hint">{{ aiHints[Math.min(Math.floor(progress / 34), 2)] }}</span>
      </div>
    </div>

    <!-- 结果展示 -->
    <div class="result-area" v-if="showResult">
      <div class="result-banner">
        <span>🎯 模拟完成！</span>
      </div>

      <div class="math-proof">
        <div class="proof-header">📐 AI推演过程</div>
        <div class="proof-body">
          <div class="proof-line proof-label">解：</div>
          <div class="proof-line step" v-for="(step, idx) in proofSteps" :key="idx">
            <span class="step-number">{{ idx + 1 }}.</span>
            <span class="step-content" v-html="step"></span>
          </div>
          <div class="proof-line proof-answer">
            <span class="answer-label">答：</span>
            <span class="answer-content">中国 &gt; 巴西 🎉</span>
          </div>
        </div>
      </div>

      <button class="restart-btn" @click="resetSimulation">
        🔄 再来一次
      </button>
    </div>

    <!-- 底部 -->
    <footer class="footer">
      <p>⚠️ 本模拟纯属娱乐，如有雷同纯属巧合</p>
    </footer>
  </div>
</template>

<script>
import { ref } from 'vue'

export default {
  name: 'App',
  setup() {
    const started = ref(false)
    const progress = ref(0)
    const showResult = ref(false)

    const aiHints = [
      '正在联网查询双方对战场次...',
      '通过AI大模型分析中...',
      '模拟即将完成...'
    ]

    const proofSteps = [
      '德国 7:1 库拉索 <span class="proof-tag">已知</span>',
      '德国 7:1 巴西 <span class="proof-tag">已知</span>',
      '库拉索 = 巴西 <span class="proof-tag">等价替换</span>',
      '中国 2:0 库拉索 <span class="proof-tag">已知</span>'
    ]

    const particleStyle = (i) => {
      const left = Math.random() * 100
      const top = Math.random() * 100
      const size = 12 + Math.random() * 20
      const delay = Math.random() * 10
      const duration = 8 + Math.random() * 12
      return {
        left: `${left}%`,
        top: `${top}%`,
        fontSize: `${size}px`,
        animationDelay: `${delay}s`,
        animationDuration: `${duration}s`
      }
    }

    const startSimulation = () => {
      started.value = true
      progress.value = 0
      const timer = setInterval(() => {
        const increment = Math.random() * 5 + 3
        progress.value = Math.min(100, progress.value + increment)
        if (progress.value >= 100) {
          progress.value = 100
          clearInterval(timer)
          setTimeout(() => {
            showResult.value = true
          }, 400)
        }
      }, 100)
    }

    const resetSimulation = () => {
      started.value = false
      progress.value = 0
      showResult.value = false
    }

    return {
      started,
      progress,
      showResult,
      aiHints,
      proofSteps,
      particleStyle,
      startSimulation,
      resetSimulation
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Noto+Sans+SC:wght@400;700;900&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Noto Sans SC', sans-serif;
  background: linear-gradient(135deg, #0a1628 0%, #1a2a4a 50%, #0d1f3c 100%);
  min-height: 100vh;
  color: #fff;
  overflow-x: hidden;
}

.app {
  max-width: 900px;
  margin: 0 auto;
  padding: 30px 20px;
  position: relative;
  min-height: 100vh;
}

/* 粒子背景 */
.particles {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 0;
  overflow: hidden;
}

.particle {
  position: absolute;
  opacity: 0.15;
  animation: float linear infinite;
}

@keyframes float {
  0%, 100% {
    transform: translateY(0) rotate(0deg);
    opacity: 0.1;
  }
  50% {
    transform: translateY(-30px) rotate(180deg);
    opacity: 0.25;
  }
}

/* 标题 */
.title-area {
  text-align: center;
  margin-bottom: 40px;
  position: relative;
  z-index: 1;
}

.main-title {
  font-size: 2.8em;
  font-weight: 900;
  background: linear-gradient(to right, #ffd700, #ff8c00, #ffd700);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
  text-shadow: none;
  filter: drop-shadow(0 0 20px rgba(255, 215, 0, 0.3));
  animation: titlePulse 3s ease-in-out infinite;
}

@keyframes titlePulse {
  0%, 100% { filter: drop-shadow(0 0 20px rgba(255, 215, 0, 0.3)); }
  50% { filter: drop-shadow(0 0 40px rgba(255, 215, 0, 0.6)); }
}

.sub-title {
  font-size: 1em;
  letter-spacing: 8px;
  color: rgba(255, 215, 0, 0.6);
  margin-top: 8px;
}

/* 对战区域 */
.vs-area {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 30px;
  margin-bottom: 50px;
  position: relative;
  z-index: 1;
}

.team {
  text-align: center;
  padding: 30px 40px;
  border-radius: 20px;
  background: rgba(255, 255, 255, 0.05);
  backdrop-filter: blur(10px);
  border: 2px solid rgba(255, 255, 255, 0.1);
  transition: all 0.6s ease;
  min-width: 200px;
}

.team-win {
  border-color: #ffd700;
  box-shadow: 0 0 30px rgba(255, 215, 0, 0.4);
  transform: scale(1.05);
}

.team-lose {
  opacity: 0.5;
  transform: scale(0.95);
  filter: grayscale(0.5);
}

.flag {
  font-size: 5em;
  line-height: 1.2;
  filter: drop-shadow(0 4px 8px rgba(0, 0, 0, 0.3));
}

.team-name {
  font-size: 1.8em;
  font-weight: 900;
  margin-top: 10px;
  letter-spacing: 4px;
}

.team-rank {
  font-size: 0.8em;
  color: rgba(255, 255, 255, 0.4);
  margin-top: 6px;
}

.vs-badge {
  position: relative;
  flex-shrink: 0;
}

.vs-text {
  font-size: 3em;
  font-weight: 900;
  color: #ff4444;
  text-shadow: 0 0 20px rgba(255, 68, 68, 0.5);
  animation: vsPulse 1.5s ease-in-out infinite;
}

.vs-glow {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 80px;
  height: 80px;
  border-radius: 50%;
  background: radial-gradient(circle, rgba(255, 68, 68, 0.2), transparent);
  animation: glowPulse 1.5s ease-in-out infinite;
}

@keyframes vsPulse {
  0%, 100% { transform: scale(1); }
  50% { transform: scale(1.2); }
}

@keyframes glowPulse {
  0%, 100% { opacity: 0.5; transform: translate(-50%, -50%) scale(1); }
  50% { opacity: 1; transform: translate(-50%, -50%) scale(1.5); }
}

/* 按钮 */
.action-area {
  text-align: center;
  position: relative;
  z-index: 1;
}

.start-btn {
  padding: 18px 50px;
  font-size: 1.3em;
  font-weight: 700;
  color: #fff;
  background: linear-gradient(135deg, #e74c3c, #c0392b);
  border: none;
  border-radius: 60px;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 8px 30px rgba(231, 76, 60, 0.4);
  display: inline-flex;
  align-items: center;
  gap: 12px;
  font-family: 'Noto Sans SC', sans-serif;
}

.start-btn:hover {
  transform: translateY(-3px) scale(1.03);
  box-shadow: 0 12px 40px rgba(231, 76, 60, 0.6);
}

.start-btn:active {
  transform: translateY(0) scale(0.98);
}

.btn-icon {
  font-size: 1.2em;
  animation: spin 2s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.disclaimer {
  margin-top: 16px;
  font-size: 0.75em;
  color: rgba(255, 255, 255, 0.3);
  font-style: italic;
}

/* 进度条 */
.progress-area {
  text-align: center;
  position: relative;
  z-index: 1;
}

.progress-container {
  width: 100%;
  max-width: 600px;
  margin: 0 auto 20px;
  height: 40px;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 20px;
  overflow: hidden;
  position: relative;
  border: 2px solid rgba(255, 215, 0, 0.3);
}

.progress-bar {
  height: 100%;
  background: linear-gradient(90deg, #1db954, #ffd700, #ff8c00);
  border-radius: 20px;
  transition: width 0.15s ease;
  position: relative;
  overflow: hidden;
}

.progress-glow {
  position: absolute;
  top: 0;
  left: -100%;
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.3), transparent);
  animation: shimmer 1.5s infinite;
}

@keyframes shimmer {
  100% { left: 100%; }
}

.progress-text {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-weight: 900;
  font-size: 1.1em;
  text-shadow: 0 1px 3px rgba(0, 0, 0, 0.5);
}

.simulating-text {
  font-size: 1.1em;
  color: rgba(255, 255, 255, 0.8);
}

.loading-dot {
  animation: blink 1s infinite;
}

@keyframes blink {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.3; }
}

.ai-hint {
  display: block;
  margin-top: 8px;
  font-size: 0.85em;
  color: rgba(255, 215, 0, 0.7);
  min-height: 1.2em;
}

/* 结果区域 */
.result-area {
  text-align: center;
  position: relative;
  z-index: 1;
  animation: fadeInUp 0.8s ease;
}

@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(30px); }
  to { opacity: 1; transform: translateY(0); }
}

.result-banner {
  font-size: 2em;
  font-weight: 900;
  color: #ffd700;
  margin-bottom: 30px;
  animation: bannerPulse 2s ease-in-out infinite;
}

@keyframes bannerPulse {
  0%, 100% { text-shadow: 0 0 10px rgba(255, 215, 0, 0.3); }
  50% { text-shadow: 0 0 30px rgba(255, 215, 0, 0.6); }
}

/* 数学证明 */
.math-proof {
  max-width: 600px;
  margin: 0 auto 30px;
  background: rgba(255, 255, 255, 0.05);
  border: 2px solid rgba(255, 215, 0, 0.3);
  border-radius: 16px;
  overflow: hidden;
  text-align: left;
}

.proof-header {
  background: linear-gradient(135deg, rgba(255, 215, 0, 0.15), rgba(255, 140, 0, 0.1));
  padding: 14px 24px;
  font-weight: 700;
  font-size: 1.1em;
  border-bottom: 1px solid rgba(255, 215, 0, 0.2);
}

.proof-body {
  padding: 20px 24px;
}

.proof-line {
  padding: 8px 0;
  font-size: 1.15em;
  line-height: 1.6;
}

.proof-label {
  font-weight: 900;
  font-size: 1.3em;
  color: #ffd700;
}

.step {
  padding-left: 20px;
  display: flex;
  align-items: center;
  gap: 8px;
  animation: stepIn 0.5s ease backwards;
}

.step:nth-child(2) { animation-delay: 0.2s; }
.step:nth-child(3) { animation-delay: 0.4s; }
.step:nth-child(4) { animation-delay: 0.6s; }
.step:nth-child(5) { animation-delay: 0.8s; }

@keyframes stepIn {
  from { opacity: 0; transform: translateX(-20px); }
  to { opacity: 1; transform: translateX(0); }
}

.step-number {
  color: rgba(255, 255, 255, 0.4);
  font-weight: 700;
  min-width: 24px;
}

.step-content {
  flex: 1;
}

.proof-tag {
  display: inline-block;
  background: rgba(29, 185, 84, 0.2);
  color: #1db954;
  font-size: 0.7em;
  padding: 2px 8px;
  border-radius: 4px;
  margin-left: 8px;
  vertical-align: middle;
}

.proof-answer {
  margin-top: 16px;
  padding-top: 16px;
  border-top: 2px dashed rgba(255, 215, 0, 0.3);
  font-size: 1.4em;
  animation: answerReveal 1s ease 1.2s backwards;
}

@keyframes answerReveal {
  from { opacity: 0; transform: scale(0.5); }
  50% { transform: scale(1.1); }
  to { opacity: 1; transform: scale(1); }
}

.answer-label {
  font-weight: 900;
  color: #ffd700;
  margin-right: 8px;
}

.answer-content {
  font-weight: 900;
  color: #1db954;
  text-shadow: 0 0 20px rgba(29, 185, 84, 0.5);
}

.restart-btn {
  padding: 12px 36px;
  font-size: 1.1em;
  font-weight: 700;
  color: #fff;
  background: rgba(255, 255, 255, 0.1);
  border: 2px solid rgba(255, 255, 255, 0.3);
  border-radius: 40px;
  cursor: pointer;
  transition: all 0.3s ease;
  font-family: 'Noto Sans SC', sans-serif;
}

.restart-btn:hover {
  background: rgba(255, 255, 255, 0.2);
  border-color: rgba(255, 215, 0, 0.5);
  transform: scale(1.05);
}

/* 底部 */
.footer {
  text-align: center;
  margin-top: 40px;
  padding: 20px;
  position: relative;
  z-index: 1;
}

.footer p {
  font-size: 0.75em;
  color: rgba(255, 255, 255, 0.2);
}

/* 响应式 */
@media (max-width: 700px) {
  .main-title { font-size: 1.8em; }
  .vs-area { gap: 15px; }
  .team { padding: 20px 25px; min-width: 140px; }
  .flag { font-size: 3.5em; }
  .team-name { font-size: 1.3em; }
  .vs-text { font-size: 2em; }
  .start-btn { padding: 14px 30px; font-size: 1.1em; }
  .proof-line { font-size: 1em; }
  .proof-answer { font-size: 1.2em; }
}
</style>
