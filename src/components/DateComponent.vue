<template>
  <div class="main">
    <div class="pannel">
      <div class="pannel-nav">
        <span @click="preMonth">&lt;</span>
        <span class="time_year">{{time.year}}年</span>
        <span class="time_month">{{time.month+1}}月</span>
        <span @click="nextMonth">&gt;</span>
        <button class="button_today" @click="handleToday()">今天</button>
      </div>
      <!-- <button class="button_today" @click="handleToday()">今天</button> -->
      <div class="pannel-content">
        <div class="days">
          <span v-for="j in 7" :key="`_`+j" class="cell">
            {{weekDays[j-1]}}
          </span>

          <div v-for="i in 5" :key="i">
            <!--
             这里是判断是不是当月内的天,不是当月的就置为灰色,
             是否是今天，做特殊标识,
             选择日期方法
             -->
            <span class="cell cell-days" v-for="j in 7" :key="j" :class="[{notCurrentMonth:!isCurrentMonth(getNowDays[(i-1)*7+(j-1)])}]">
              <span class="context_text" :class="[{today:isToday(getNowDays[(i-1)*7+(j-1)])}]">
                {{getNowDays[(i-1)*7+(j-1)].getDate()}}
              </span>
            </span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import * as utils from "../../utils/util"
  export default {
    name: "DateComponent",
    data() {
      let {
        year,
        month
      } = utils.getYearMonthDay(this.value);
      return {
        weekDays: ['星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六'],
        time: {
          year,
          month
        },
      }
    },
    props: {
      value: {
        type: Date,
        default: () => new Date()
      }
    },
    methods: {
      isCurrentMonth(date) {
        // alert(data.month);
        //判断是不是当前月内的日期
        let {
          year,
          month
        } = utils.getYearMonthDay(this.value);
        utils.getDate(this.time.year, this.time.month)
        let {
          year: y,
          month: m
        } = utils.getYearMonthDay(date);
        console.log(year);
        console.log(y);
        console.log(month);
        console.log(m);
        return year === y && month === m;
      },
      isToday(date) {
        //判断是否是今天
        let {
          year,
          month,
          day
        } = utils.getYearMonthDay(new Date());
        let {
          year: y,
          month: m,
          day: d
        } = utils.getYearMonthDay(date);
        return year === y && month === m && day === d;
      },
      //上一个月
      preMonth() {
        let d = utils.getDate(this.time.year, this.time.month, 1);
        d.setMonth(d.getMonth() - 1);
        this.time = utils.getYearMonthDay(d);
      },
      //下一个月
      nextMonth() {
        let d = utils.getDate(this.time.year, this.time.month, 1);
        d.setMonth(d.getMonth() + 1);
        this.time = utils.getYearMonthDay(d);
      },
      // 点击回到今天
      handleToday() {
        this.time = utils.getYearMonthDay(new Date());
      },
      // chooseDate(date){
      //   alert(date);
      // }
    },
    mounted() {
      console.log(this.getNowDays);
    },
    computed: {
      getNowDays() {
        //获取当前时间
        let {
          year,
          month
        } = utils.getYearMonthDay(utils.getDate(this.time.year, this.time.month, 1));
        //得到当前月的第一天
        let currentFirstDay = utils.getDate(year, month, 1);
        //当前天是星期几
        let week = currentFirstDay.getDay();
        //当前开始的天数
        let startDay = currentFirstDay - week * 60 * 60 * 1000 * 24;
        //循环35天
        let arr = [];
        for (let i = 0; i < 35; i++) {
          arr.push(new Date(startDay + i * 60 * 60 * 1000 * 24));
        }
        return arr;
      },
    }
  }
</script>

<style>
  .pannel {
    position: absolute;
    margin-left: 28%;
    margin-top: 50px;
    background-color: ghostwhite;
    width: 90*7px;
  }

  .pannel-nav {
    display: flex;
    margin-left: 32%;
    /* justify-content: center; */
    margin-top: 20px;
    height: 40px;
  }

  .time_year {
    margin-left: 20px;
  }

  .time_month {
    margin-right: 20px;
  }

  .button_today {
    margin-left: 240px;
    width: 50px;
    height: 25px;
  }

  .pannel-content .cell {
    display: inline-flex;
    justify-content: center;
    /* align-items: center; */
    width: 100px;
    height: 80px;
    font-weight: bold;
    box-sizing: border-box;
  }

  .pannel-content .cell-days:active {
    border: 1px solid white;
    background-color: #42B983;
  }

  .active {
    background-color: aqua;
  }

  .pannel-content .cell-days {
    border: 1px solid grey;
    /* position:relative; */
  }

  .context_text {
    margin-right: 0px;
    margin-left: 65px;
    margin-top: 5px;
    margin-bottom: 45px;
  }

  .notCurrentMonth {
    color:dimgray;
  }

  .today {
    width: 22px;
    height: 22px;
    background-color: deepskyblue;
    color: white;
    border-radius: 100%;
    /* border: 1px solid grey; */
  }
</style>
