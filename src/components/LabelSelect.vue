<template>
  <div class="label-select-wrap">
    <span class="label">{{ label + ':' }}</span>
    <el-select v-model="value" size="small" class="m-2 soft-select" :placeholder="placeholder" width="80" clearable>
      <el-option
        v-for="item in options"
        :key="item.value"
        :label="item.label"
        :value="item.value"
      />
    </el-select>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'
const value = ref()
const emits = defineEmits(['changeValue'])
defineProps({
  label: {
    type: String,
    default: '选项名称'
  },
  options: {
    type: Array,
    default: () => {
      return [
        { value: 'option1', label: 'option1' },
        { value: 'option2', label: 'option2' }
      ]
    }
  },
  placeholder: {
    type: String,
    default: '请选择'
  }
})
watch(
  () => value.value,
  (newValue) => {
    emits('changeValue', newValue)
  }
)
</script>

<style lang="scss" scoped>
.label-select-wrap {
  display: flex;
  align-items: center;
  height: 40px;
  padding: 0 10px;
  border-radius: 999px;
  background: linear-gradient(145deg, #edf2f9 0%, #e8eef7 100%);
  box-shadow:
    6px 6px 12px rgba(161, 177, 201, 0.24),
    -6px -6px 12px rgba(255, 255, 255, 0.9);
}

.label {
  font-size: 14px;
  font-family:
    PingFangSC-Regular,
    PingFang SC;
  font-weight: 400;
  color: #3f5f8f;
  line-height: 20px;
  margin-right: 10px;
}

.m-2 {
  flex: 1
}

:deep(.soft-select .el-input__wrapper) {
  border-radius: 10px;
  background: linear-gradient(145deg, #eef3fa 0%, #e9eef7 100%);
  box-shadow:
    inset 2px 2px 5px rgba(166, 181, 203, 0.3),
    inset -2px -2px 5px rgba(255, 255, 255, 0.88);
}
</style>
