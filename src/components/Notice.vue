<script>
  import dayjs from 'dayjs';
  export default {
    data() {
      return {
        studentName: '',
        studentId: '',
        studentClass: '',
        remainRequestNum: 0,
        notice: {
          title: [],
          content: [],
          time: [],
          user: [],
        },
      }
    },
    mounted() {
      this.getStudentData();
    },
    methods: {
      getStudentData() {
        this.studentName = '姓名'
        console.log(this.studentName)
        this.studentId = '学号'
        this.studentClass = '班级'
        this.remainRequestNum = 1
        this.notice = {
          title: ['公告1','公告2','公告3'],
          content: ['这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条\n这里没有滚动条','测试','滚动条测试\n滚动条测试\n滚动条测试\n滚动条测试\n滚动条测试\n滚动条测试\n滚动条测试\n滚动条测试\n滚动条测试\n滚动条测试\n滚动条测试\n滚动条测试\n滚动条测试\n滚动条测试\n滚动条测试\n滚动条测试\n滚动条测试'],
          time: ['2024-09-01T00:00:00+08:00','2024-10-01T00:00:00+08:00','2024-11-01T00:00:00+08:00'],
          user: ['admin1','admin2','admin3']
        }
      },
      Lines(i) {
        return this.notice.content[i].split('\n')
      },
      formatDate(date) {
        return dayjs(date).format('YYYY年MM月DD日 HH:mm:ss')
      }
    }
  }
</script>
<template>
  <div class="NewNotice">
    <h3>最新公告</h3>
    <h4 style="text-align: center;">{{ notice.title[notice.title.length-1] }}</h4>
    <p style="text-align: center;">时间：{{ formatDate(notice.time[notice.time.length-1]) }} 发起人：{{ notice.user[notice.user.length-1] }}</p>
    <p v-for="line in Lines(notice.content.length-1)">{{ line }}</p>
  </div>
</template>

<style scoped>
  .NewNotice {
    width: 100%;
    height: 400px;
    box-shadow: 0px 0px 0px #ccc;
    border-radius: 10px;
    border: 3px solid #00afee;
    padding: 10px;
    line-height: 2;
    margin: 5px;
    white-space: wrap;
    overflow: auto;
  }
</style>