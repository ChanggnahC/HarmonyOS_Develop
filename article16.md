# 鸿蒙Next开发入门教程--公历日期转农历日期
上周为大家分享了月历组件的实现方法，考虑到有些友友需要在月历中显示农历，所以今天再分享一下如何在鸿蒙开发中进行阳历和农历的转换。

其实在鸿蒙的ohpm仓库有一些开源的框架，可以直接安装使用，如果大家对转换的过程感兴趣可以继续往下看。

阳历日期向农历日期的直接转换是不太容易的，我们的思路是这样：阳历1900年1月31日是农历1900年正月初一，先计算需要转换的阳历日期距离1900年1月31日的天数，然后用农历1900年正月初一加上这个天数差值推算出农历日期。计算的过程中要考虑到农历的闰月和每个月的天数。具体代码如下：

```
export class LunarClass {

  // 农历查询表
  static lunarInfo = [0x04bd8, 0x04ae0, 0x0a570, 0x054d5, 0x0d260, 0x0d950, 0x16554, 0x056a0, 0x09ad0, 0x055d2, // 1900-1909

    0x04ae0, 0x0a5b6, 0x0a4d0, 0x0d250, 0x1d255, 0x0b540, 0x0d6a0, 0x0ada2, 0x095b0, 0x14977, // 1910-1919

    0x04970, 0x0a4b0, 0x0b4b5, 0x06a50, 0x06d40, 0x1ab54, 0x02b60, 0x09570, 0x052f2, 0x04970, // 1920-1929

    0x06566, 0x0d4a0, 0x0ea50, 0x06e95, 0x05ad0, 0x02b60, 0x186e3, 0x092e0, 0x1c8d7, 0x0c950, // 1930-1939

    0x0d4a0, 0x1d8a6, 0x0b550, 0x056a0, 0x1a5b4, 0x025d0, 0x092d0, 0x0d2b2, 0x0a950, 0x0b557, // 1940-1949

    0x06ca0, 0x0b550, 0x15355, 0x04da0, 0x0a5b0, 0x14573, 0x052b0, 0x0a9a8, 0x0e950, 0x06aa0, // 1950-1959

    0x0aea6, 0x0ab50, 0x04b60, 0x0aae4, 0x0a570, 0x05260, 0x0f263, 0x0d950, 0x05b57, 0x056a0, // 1960-1969

    0x096d0, 0x04dd5, 0x04ad0, 0x0a4d0, 0x0d4d4, 0x0d250, 0x0d558, 0x0b540, 0x0b6a0, 0x195a6, // 1970-1979

    0x095b0, 0x049b0, 0x0a974, 0x0a4b0, 0x0b27a, 0x06a50, 0x06d40, 0x0af46, 0x0ab60, 0x09570, // 1980-1989

    0x04af5, 0x04970, 0x064b0, 0x074a3, 0x0ea50, 0x06b58, 0x05ac0, 0x0ab60, 0x096d5, 0x092e0, // 1990-1999

    0x0c960, 0x0d954, 0x0d4a0, 0x0da50, 0x07552, 0x056a0, 0x0abb7, 0x025d0, 0x092d0, 0x0cab5, // 2000-2009

    0x0a950, 0x0b4a0, 0x0baa4, 0x0ad50, 0x055d9, 0x04ba0, 0x0a5b0, 0x15176, 0x052b0, 0x0a930, // 2010-2019

    0x07954, 0x06aa0, 0x0ad50, 0x05b52, 0x04b60, 0x0a6e6, 0x0a4e0, 0x0d260, 0x0ea65, 0x0d530, // 2020-2029

    0x05aa0, 0x076a3, 0x096d0, 0x04afb, 0x04ad0, 0x0a4d0, 0x1d0b6, 0x0d250, 0x0d520, 0x0dd45, // 2030-2039

    0x0b5a0, 0x056d0, 0x055b2, 0x049b0, 0x0a577, 0x0a4b0, 0x0aa50, 0x1b255, 0x06d20, 0x0ada0, // 2040-2049

    0x14b63, 0x09370, 0x049f8, 0x04970, 0x064b0, 0x168a6, 0x0ea50, 0x06b20, 0x1a6c4, 0x0aae0, // 2050-2059

    0x0a2e0, 0x0d2e3, 0x0c960, 0x0d557, 0x0d4a0, 0x0da50, 0x05d55, 0x056a0, 0x0a6d0, 0x055d4, // 2060-2069

    0x052d0, 0x0a9b8, 0x0a950, 0x0b4a0, 0x0b6a6, 0x0ad50, 0x055a0, 0x0aba4, 0x0a5b0, 0x052b0, // 2070-2079

    0x0b273, 0x06930, 0x07337, 0x06aa0, 0x0ad50, 0x14b55, 0x04b60, 0x0a570, 0x054e4, 0x0d160, // 2080-2089

    0x0e968, 0x0d520, 0x0daa0, 0x16aa6, 0x056d0, 0x04ae0, 0x0a9d4, 0x0a2d0, 0x0d150, 0x0f252, // 2090-2099

    0x0d520] // 2100


  static solarMonth = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]

  //农历y年闰月的天数
  static leapDays(y) {
    if (this.leapMonth(y)) {
      return ((this.lunarInfo[y - 1900] & 0x10000) ? 30 : 29)
    }
    return (0)
  }


  //农历y年闰月是哪个月
  static leapMonth(y) { // 闰字编码 \u95f0
    return (this.lunarInfo[y - 1900] & 0xf)
  }

  //农历y年一整年的总天数
  static lYearDays(y) {
    var i
    var sum = 348
    for (i = 0x8000; i > 0x8; i >>= 1) {
      sum += (this.lunarInfo[y - 1900] & i) ? 1 : 0
    }
    return (sum + this.leapDays(y))
  }

  //农历y年m月（非闰月）的总天数
  static monthDays(y, m) {
    if (m > 12 || m < 1) {
      return -1
    } // 月份参数从1至12，参数错误返回-1
    return ((this.lunarInfo[y - 1900] & (0x10000 >> m)) ? 30 : 29)
  }


  static  solar2lunar(y, m, d) { // 参数区间1900.1.31~2100.12.31

    // 年份限定、上限
    if (y < 1900 || y > 2100) {
      return -1 // undefined转换为数字变为NaN
    }

    // 公历传参最下限
    if (y === 1900 && m === 1 && d < 31) {
      return -1
    }

    // 未传参  获得当天
    var objDate = null

    if (!y) {
      objDate = new Date()
    } else {
      objDate = new Date(y, parseInt(m) - 1, d)
    }

    var i
    var leap = 0
    var temp = 0

    // 修正ymd参数
    y = objDate.getFullYear()
    m = objDate.getMonth() + 1
    d = objDate.getDate()

    var offset = (Date.UTC(objDate.getFullYear(), objDate.getMonth(), objDate.getDate()) - Date.UTC(1900, 0, 31)) / 86400000

    for (i = 1900; i < 2101 && offset > 0; i++) {
      temp = this.lYearDays(i)
      offset -= temp
    }

    if (offset < 0) {
      offset += temp;
      i--
    }

    // 是否今天
    var isTodayObj = new Date()
    var isToday = false

    if (isTodayObj.getFullYear() === y && isTodayObj.getMonth() + 1 === m && isTodayObj.getDate() === d) {
      isToday = true
    }

    // 星期几
    var nWeek = objDate.getDay()

    // 数字表示周几 顺应周一开始的惯例
    if (nWeek === 0) {
      nWeek = 7
    }

    // 农历年
    var year = i
    leap = this.leapMonth(i) // 闰哪个月
    var isLeap = false

    // 效验闰月
    for (i = 1; i < 13 && offset > 0; i++) {
      // 闰月
      if (leap > 0 && i === (leap + 1) && isLeap === false) {
        --i

        isLeap = true;
        temp = this.leapDays(year) // 计算农历闰月天数
      } else {
        temp = this.monthDays(year, i) // 计算农历普通月天数
      }

      // 解除闰月
      if (isLeap === true && i === (leap + 1)) {
        isLeap = false
      }
      offset -= temp
    }

    // 闰月导致数组下标重叠取反
    if (offset === 0 && leap > 0 && i === leap + 1) {
      if (isLeap) {
        isLeap = false
      } else {
        isLeap = true;
        --i
      }
    }

    if (offset < 0) {
      offset += temp;
      --i
    }

    // 农历月
    var month = i

    // 农历日
    var day = offset + 1

    return {
      'lYear': year,
      'lMonth': month,
      'lDay': day,
      'cYear': y,
      'cMonth': m,
      'cDay': d,
    }
  }
}
```
