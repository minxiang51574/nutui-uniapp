<script lang="ts">
import { reactive, ref, toRefs } from 'vue'

export default {
  props: {},
  setup() {
    const show = ref(false)
    const popupDesc = ref()
    const msg = ref()
    const showToast = ref(false)

    const CurrentDate = reactive({
      currentDate: new Date(2022, 4, 10, 10, 10),
      currentDate2: new Date(2022, 4, 10, 10, 10),
      currentDate3: new Date(2022, 4, 10, 10, 10),
      currentDate4: new Date(2022, 4, 10, 10, 10),
      currentDate5: new Date(2022, 4, 10, 10, 10),
      currentDate6: new Date(2022, 4, 10, 10, 10),
      currentDate7: new Date(2022, 4, 10, 0, 0),
    })

    const formatter = (type: string, option: any) => {
      switch (type) {
        case 'year':
          option.text += ''
          break
        case 'month':
          option.text += '月'
          break
        case 'day':
          option.text += '日'
          break
        case 'hour':
          option.text += '时'
          break
        case 'minute':
          option.text += '分'
          break
        default:
          option.text += ''
      }
      return option
    }

    const formatter1 = (type: string, option: any) => {
      switch (type) {
        case 'year':
          option.text += '年'
          break
        case 'month':
          option.text += '月'
          break
        case 'day':
          option.text += '日'
          break
        case 'hour':
          option.text += '时'
          break
        default:
          option.text += ''
      }
      return option
    }

    const filter = (type: string, options: any[]) => {
      if (type === 'hour')
        return options.filter((option: { value: any }) => Number(option.value) % 6 === 0)

      return options
    }

    const confirm = ({
      selectedOptions,
    }: {
      selectedValue: (string | number)[]
      selectedOptions: any
    }) => {
      showToast.value = true
      msg.value = selectedOptions.map((val: any) => val.text).join('-')
    }
    const popupConfirm = ({ selectedOptions }: { selectedValue: string[], selectedOptions: any }) => {
      popupDesc.value = selectedOptions.map((val: any) => val.text).join('')
      show.value = false
    }
    const alwaysFun = () => {
      popupDesc.value = '永远有效'
      show.value = false
    }
    return {
      show,
      msg,
      showToast,
      popupDesc,
      ...toRefs(CurrentDate),
      minDate: new Date(2020, 0, 1),
      maxDate: new Date(2025, 10, 1),
      confirm,
      formatter,
      formatter1,
      filter,
      alwaysFun,
      popupConfirm,
    }
  },
}
</script>

<template>
  <div class="demo">
    <h2 class="title">
      选择年月日
    </h2>
    <!-- 选择年月日 -->
    <nut-date-picker
      v-model="currentDate"
      :min-date="minDate"
      :max-date="maxDate"
      :three-dimensional="false"
      :is-show-chinese="true"
      @confirm="confirm"
    />

    <h2 class="title">
      配合 Popup 使用
    </h2>
    <nut-cell title="选择日期" :desc="popupDesc" @click="show = true" />
    <nut-popup v-model:visible="show" safe-area-inset-bottom position="bottom">
      <nut-date-picker
        v-model="currentDate"
        :min-date="minDate"
        :max-date="maxDate"
        :is-show-chinese="true"
        :three-dimensional="false"
        @confirm="popupConfirm"
        @cancel="show = false"
      >
        <nut-button block type="primary" @click="alwaysFun">
          永远有效
        </nut-button>
      </nut-date-picker>
    </nut-popup>

    <h2 class="title">
      选择月日
    </h2>
    <!-- 选择月日 -->
    <nut-date-picker
      v-model="currentDate2"
      type="month-day"
      title="日期选择"
      :min-date="new Date(2022, 0, 1)"
      :max-date="new Date(2022, 7, 1)"
      @confirm="confirm"
    />
    <h2 class="title">
      选择年月日时分
    </h2>

    <!-- 选择年月日时分 -->
    <nut-date-picker
      v-model="currentDate3"
      title="日期时间选择"
      type="datetime"
      :min-date="minDate"
      :max-date="maxDate"
      @confirm="confirm"
    />
    <h2 class="title">
      选择时分秒
    </h2>
    <!-- 选择时分秒 -->
    <nut-date-picker
      v-model="currentDate4"
      title="时间选择"
      type="time"
      :min-date="minDate"
      :max-date="maxDate"
      @confirm="confirm"
    />
    <h2 class="title">
      选择时分
    </h2>
    <!-- 选择时分 -->
    <nut-date-picker
      v-model="currentDate4"
      title="时间选择"
      type="hour-minute"
      :min-date="minDate"
      :max-date="maxDate"
      @confirm="confirm"
    />
    <h2 class="title">
      格式化选项
    </h2>
    <!-- 格式化选项 -->
    <nut-date-picker
      v-model="currentDate5"
      title="日期选择"
      type="datetime"
      :min-date="new Date(2022, 0, 1)"
      :max-date="new Date(2022, 10, 1)"
      :formatter="formatter"
      @confirm="confirm"
    />
    <h2 class="title">
      分钟数递增步长设置
    </h2>
    <!-- 分钟数递增步长设置 -->
    <nut-date-picker
      v-model="currentDate6"
      title="时间选择"
      type="time"
      :min-date="minDate"
      :max-date="maxDate"
      :minute-step="5"
      @confirm="confirm"
    />
    <h2 class="title">
      过滤选项
    </h2>

    <!-- 过滤选项 -->
    <nut-date-picker
      v-model="currentDate7"
      title="时间选择"
      type="datehour"
      :min-date="minDate"
      :max-date="maxDate"
      :filter="filter"
      :formatter="formatter1"
      @confirm="confirm"
    />

    <nut-toast v-model:visible="showToast" :msg="msg" type="text" />
  </div>
</template>

<style lang="scss" scoped></style>

<route lang="json">
{
  "style": {
    "navigationBarTitleText": "DatePicker"
  }
}
</route>
