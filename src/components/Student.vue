<template>
    <div class="container">
      <!-- 侧边导航栏 -->
      <div class="sidebar">
        <div class="nav-item" 
             v-for="(item, index) in modules" 
             :key="index"
             @click="currentModule = item">
          {{ item.name }}
        </div>
      </div>
  
      <!-- 主要内容区域 -->
      <div class="content">
        <div class="module" v-if="currentModule">
          <!-- 总览模块 -->
          <div v-if="currentModule.name === '总览'" class="module">
            <div v-for="module in modules" :key="module.name" class="overview-section">
              <h3 class="overview-title">{{ module.name }}</h3>
              <div v-if="module.content" class="overview-content">
                {{ module.content }}
              </div>
              <div v-else class="empty-message">
                暂无内容
              </div>
            </div>
          </div>
  
          <!-- 原有的模块内容 -->
          <div v-else>
            <h2>{{ currentModule.name }}</h2>
            <textarea v-model="currentModule.content" 
                      placeholder="请在这里输入内容..."></textarea>
            <button @click="saveContent">保存</button>
  
            <div class="history-list" v-if="currentModule.history && currentModule.history.length">
              <h3>历史记录</h3>
              <div class="history-item" v-for="(item, index) in sortedHistory" :key="index">
                <div class="history-meta">
                  #{{index + 1}} - 提交时间：{{ formatDate(item.timestamp) }}
                </div>
                <div>{{ item.content }}</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        currentModule: null,
        modules: [
          {
            name: '总览',
            content: '',
            history: []
          },
          {
            name: '个人计划',
            content: '',
            history: []
          },
          {
            name: '个人奖项',
            content: '',
            history: []
          },
          {
            name: '心得体会',
            content: '',
            history: []
          }
        ]
      };
    },
    computed: {
      sortedHistory() {
        return this.currentModule.history
          ? [...this.currentModule.history].sort((a, b) => b.timestamp - a.timestamp)
          : [];
      }
    },
    mounted() {
      // 默认显示总览模块
      this.currentModule = this.modules[0];
      // 从服务器加载数据
      this.loadContent();
    },
    methods: {
      formatDate(timestamp) {
        return new Date(timestamp).toLocaleString();
      },
      async saveContent() {
        try {
          // 添加历史记录
          if (!this.currentModule.history) {
            this.currentModule.history = [];
          }
  
          this.currentModule.history.push({
            content: this.currentModule.content,
            timestamp: Date.now()
          });
  
          // 发送数据到服务器
          const response = await fetch('/api/student/modules', {
            method: 'POST',
            headers: {
              'Content-Type': 'application/json',
              'Authorization': 'Bearer ' + localStorage.getItem('token')
            },
            body: JSON.stringify(this.modules)
          });
  
          if (!response.ok) {
            throw new Error('保存失败');
          }
  
          alert('保存成功！');
        } catch (error) {
          alert('保存失败：' + error.message);
          console.error('保存失败：', error);
        }
      },
      async loadContent() {
        try {
          // 从服务器获取数据
          const response = await fetch('/api/student/modules', {
            method: 'GET',
            headers: {
              'Authorization': 'Bearer ' + localStorage.getItem('token')
            }
          });
  
          if (!response.ok) {
            throw new Error('加载失败');
          }
  
          const data = await response.json();
          this.modules = data;
        } catch (error) {
          alert('加载失败：' + error.message);
          console.error('加载失败：', error);
        }
      }
    }
  };
  </script>
  
  <style scoped>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  
  .container {
    display: flex;
    min-height: 100vh;
  }
  
  .sidebar {
    width: 200px;
    background-color: #2c3e50;
    padding: 20px;
    color: white;
  }
  
  .nav-item {
    padding: 10px;
    margin: 5px 0;
    cursor: pointer;
    border-radius: 5px;
    transition: background-color 0.3s;
  }
  
  .nav-item:hover {
    background-color: #34495e;
  }
  
  .content {
    flex: 1;
    padding: 20px;
  }
  
  .module {
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  }
  
  textarea {
    width: 100%;
    min-height: 200px;
    margin: 10px 0;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 4px;
  }
  
  button {
    background-color: #3498db;
    color: white;
    border: none;
    padding: 8px 16px;
    border-radius: 4px;
    cursor: pointer;
  }
  
  button:hover {
    background-color: #2980b9;
  }
  
  .history-list {
    margin-top: 20px;
    border-top: 1px solid #ddd;
    padding-top: 20px;
  }
  
  .history-item {
    padding: 10px;
    border: 1px solid #eee;
    margin-bottom: 10px;
    border-radius: 4px;
  }
  
  .history-meta {
    font-size: 0.9em;
    color: #666;
    margin-bottom: 5px;
  }
  
  .overview-section {
    margin-bottom: 30px;
  }
  
  .overview-title {
    font-size: 1.2em;
    color: #2c3e50;
    margin-bottom: 10px;
    padding-bottom: 5px;
    border-bottom: 2px solid #3498db;
  }
  
  .overview-content {
    background-color: #f9f9f9;
    padding: 15px;
    border-radius: 4px;
    white-space: pre-wrap;
  }
  
  .empty-message {
    color: #666;
    font-style: italic;
  }
  </style>
  