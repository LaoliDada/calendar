<template>
    <div id="calendar">
        <!-- 日期 -->
        <ul class="days">
            <li v-for="item in theWeekArr" class="week"><!-- 循环渲染每一周 -->
                <ul class="day">
                    <li v-for="dayIte in item"><span :class="theUserYear==year&&theUserMonth==month&&day==dayIte?'theDay':''">{{dayIte}}</span></li><!-- 渲染每一天,class判断为当天，可根据该判断执行其它操作 -->
                </ul>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    data: function () {
        return {
            year:new Date().getFullYear(),//当前用户系统年份，用作判断
            month:new Date().getMonth()+1,//当前用户系统年月份，用作判断
            day:new Date().getDay(),//当前用户系统年日，用作判断
            dayArr: [],//没用，写错了
            theWeekArr:[],//日期数组
            theUserYear:"2012",//需要渲染的年份
            theUserMonth:"2"//需要渲染的月份
        }
    },
    created() {
        let that = this;
        that.initDateArr(this.theUserYear,this.theUserMonth);//执行主函数
    },
    methods: {
       initDateArr(year,month){
           let that = this;
           let theDateLastDay = new Date(new Date(that.formatDate(year,Number(month)+1,1)).getTime()-86400000);//获取该月最后一天
           let lastWeek = theDateLastDay.getDay()+1;//获取当前月最后一周天数
           let firstWeek = 7-new Date(that.formatDate(year,month,1)).getDay();//获取当前月第一周天数
           let theMonthDaySum = theDateLastDay.getDate();//当前月总天数
           let theMonthWeekSun = (theMonthDaySum-(firstWeek+lastWeek))/7+2;//当前月在日历上总跨度周数
           let runDay = 0;//日期增值
           for(let i = 0;i<theMonthWeekSun;i++){//循环总周数
               that.theWeekArr.push([]);
               for(let k = 0,kSun = 7;k<kSun;k++){
                   if(i==0){//第一周
                       if(k >= new Date(that.formatDate(year,month,1)).getDay()){//是否是开始第一天，若小于第一天则push空值
                        runDay++;
                        that.theWeekArr[i].push(runDay);
                       }else{
                        that.theWeekArr[i].push("");
                       }
                   }else if(i==theMonthWeekSun-1){//最后一周
                       if(runDay<theDateLastDay.getDate()){//是否是最后一天，若大于最后一天，则push空值
                           runDay++;
                          that.theWeekArr[i].push(runDay);
                       }else{
                            that.theWeekArr[i].push("");
                       }
                   }else{//其它周正常插入
                       runDay++;
                       that.theWeekArr[i].push(runDay);
                   }
               }
           }
        //    let theDateLastDay = new Date(theDay);
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
li{
    list-style: none;
}
.week{
    ul{
        display:flex;
        justify-content: space-between;
        width:100%;
    }
    .theDay{
        color:#f00;
    }
}

</style>
