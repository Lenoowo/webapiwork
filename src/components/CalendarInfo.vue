<template>
    <h2>今日黄历</h2>
    <div v-if="date" class="calendar-info">
      <p><strong>阳历:</strong> {{ calendarData.yangli }}</p>
      <p><strong>阴历:</strong> {{ calendarData.yinli }}</p>
      <p><strong>五行:</strong> {{ calendarData.wuxing }}</p>
      <p><strong>冲煞:</strong> {{ calendarData.chongsha }}</p>
      <p><strong>百吉:</strong> {{ calendarData.baiji }}</p>
      <p><strong>吉神:</strong> {{ calendarData.jishen }}</p>
      <p><strong>宜:</strong> {{ calendarData.yi }}</p>
      <p><strong>凶神:</strong> {{ calendarData.xiongshen }}</p>
      <p><strong>忌:</strong> {{ calendarData.ji }}</p>
    </div>
    <div v-if="error">
      <p style="color: red;">{{ error }}</p>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      date: {
        type: String,
        required: true,
      },
    },
    data() {
      return {
        calendarData: {},
        error: null, // 存储错误信息
      };
    },
    methods: {
      fetchCalendar() {
        const apiKey = '9e25221b1e04c92ecf6d3ce58e44f4ac'; // 请替换为你自己的API密钥
        const formattedDate = this.date; // 从props获取的日期
        this.error = null; // 重置错误信息
  
        // 使用完整的 API URL
        fetch(`/calendarApi/laohuangli/d?key=${apiKey}&date=${formattedDate}`)
          .then((response) => {
            if (!response.ok) {
              throw new Error('网络响应不正常');
            }
            return response.json();
          })
          .then((data) => {
            if (data.error_code === 0) {
              this.calendarData = data.result; // 设置黄历数据
            } else {
              //this.error = `获取失败: ${data.reason}`; // 设置错误信息
            }
          })
          .catch((error) => {
           // this.error = `请求失败: ${error.message}`; // 捕获请求错误
          });
      },
    },
    watch: {
      date: 'fetchCalendar', // 当日期变化时，重新获取黄历
    },
    mounted() {
      this.fetchCalendar(); // 组件挂载后获取黄历
    },
  };
  </script>
  
  <style scoped>
  .calendar-info {
    line-height: 0.9; /* 设置行距 */
    font-size: 16px; /* 设置字体大小 */
    text-align: left; /* 使文本靠左对齐 */
  }
  </style>
  