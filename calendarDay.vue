<template>
    <div id="calendar">
        <!-- 日期 -->
        <ul class="days">
            <!-- 核心 v-for循环 每一次循环用<li>标签创建一天 -->
            <li v-for="dayobject in days">
                <!--本月-->
                <!--如果不是本月  改变类名加灰色-->
                <!-- <span v-if="dayobject.day.getMonth()+1 != currentMonth" class="other-month">{{ dayobject.day.getDate() }}</span> -->

                <!--如果是本月  还需要判断是不是这一天-->
                <div v-if="dayobject.day.getMonth()+1 == currentMonth">
                    <!--今天  同年同月同日-->
                    <span v-if="dayobject.day.getFullYear() == new Date().getFullYear() && dayobject.day.getMonth() == new Date().getMonth() && dayobject.day.getDate() == new Date().getDate()" class="active">{{ dayobject.day.getDate() }}</span>
                    <span v-else>{{ dayobject.day.getDate() }}</span>
                </div>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    data: function () {
        return {
            currentDay: 1,
            currentMonth: 1,
            currentYear: 1970,
            currentWeek: 1,
            days: [],
        }
    },
    created() {
        let that = this;
        that.initData(null);
    },
    methods: {
        initData: function (cur) {
            let that = this;
            let leftcount = 0; //存放剩余数量
            let date;
            if (cur) {
                date = new Date(cur);
            } else {
                let now = new Date();
                let d = new Date(that.formatDate(now.getFullYear(), now.getMonth(), 1));
                d.setDate(35);
                date = new Date(that.formatDate(d.getFullYear(), d.getMonth() + 1, 1));
            }
            that.currentDay = date.getDate();
            that.currentYear = date.getFullYear();
            that.currentMonth = date.getMonth() + 1;
            that.currentWeek = date.getDay(); // 1...6,0
            if (that.currentWeek == 0) {
                that.currentWeek = 7;
            }
            let str = that.formatDate(that.currentYear, that.currentMonth, that.currentDay);
            that.days.length = 0;
            // 今天是周日，放在第一行第1个位置
            //初始化本周

            for (let i = that.currentWeek; i >= 0; i--) {
                let d = new Date(str);
                d.setDate(d.getDate() - i);
                let dayobject = {}; //用一个对象包装Date对象  以便为以后预定功能添加属性
                dayobject.day = d;
                that.days.push(dayobject); //将日期放入data 中的days数组 供页面渲染使用
            }

            //其他周
            for (let i = 1; i <= 35 - that.currentWeek; i++) {
                let d = new Date(str);
                d.setDate(d.getDate() + i);
                let dayobject = {};
                dayobject.day = d;
                console.log(new Date(d).getDate())
                that.days.push(dayobject);
            }
        },
        pickPre: function (year, month) {
            let that = this;
            // setDate(0); 上月最后一天
            // setDate(-1); 上月倒数第二天
            // setDate(dx) 参数dx为 上月最后一天的前后dx天
            let d = new Date(that.formatDate(year, month, 1));
            d.setDate(0);
            that.initData(that.formatDate(d.getFullYear(), d.getMonth() + 1, 1));
        },
        pickNext: function (year, month) {
            let that = this;
            let d = new Date(that.formatDate(year, month, 1));
            d.setDate(35);
            that.initData(that.formatDate(d.getFullYear(), d.getMonth() + 1, 1));
        },
        pickYear: function (year, month) {
            alert(year + "," + month);
        },
        // 返回 类似 2016-01-02 格式的字符串
        formatDate: function (year, month, day) {
            let y = year;
            let m = month;
            if (m < 10) m = "0" + m;
            let d = day;
            if (d < 10) d = "0" + d;
            return y + "-" + m + "-" + d
        },
    }
}
</script>
<style scoped lang="scss">
/*日历*/
#calendar {
  width: 100%;
  //   height: 4.7rem;
  background: #fff;
}

.days {
  display: flex;
  flex-wrap: wrap;
  //   justify-content: space-around;
  li {
    display: inline-block;
    width: 14.2%;
    height: 0.8rem;
    text-align: center;
    line-height: 0.8rem;
    font-size: 0.26rem;
    color: #333;
    div {
      span {
        height: 0.48rem;
        width: 0.48rem;
        line-height: 0.48rem;
        display: inline-block;
        &.active {
          background: #f05858;
          border-radius: 50%;
          color: #fff;
        }
      }
    }
  }
}
</style>
