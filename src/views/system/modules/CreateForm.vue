<template>
  <a-modal
    title="新建规则"
    :width="640"
    :visible="visible"
    :confirmLoading="loading"
    @ok="() => { $emit('ok') }"
    @cancel="() => { $emit('cancel') }"
  >
    <a-spin :spinning="loading">
     <a-form  :form="form" v-bind="formLayout">
          <a-row :gutter="48">
            <a-col :md="12" :sm="12">
              <a-form-item label="参数代码">
                <a-input v-decorator="['code', { initialValue: 0 }]"  placeholder="" />
              </a-form-item>
            </a-col>
             <a-col :md="12" :sm="12">
                <a-form-item label="参数名称">
                  <a-input v-decorator="['name', { initialValue: 0 }]" style="width: 100%" />
                </a-form-item>
              </a-col>
            <template v-if="advanced">
              <a-col :md="8" :sm="24">
              <a-form-item label="使用状态">
                <a-select v-decorator="['status', { initialValue: 0 }]" placeholder="请选择" default-value="0">
                  <a-select-option value="0">全部</a-select-option>
                  <a-select-option value="1">关闭</a-select-option>
                  <a-select-option value="2">运行中</a-select-option>
                </a-select>
              </a-form-item>
            </a-col>
            </template>
          </a-row>
        </a-form>
    </a-spin>
  </a-modal>
</template>

<script>
import pick from 'lodash.pick'

// 表单字段
const fields = ['description', 'id']

export default {
  props: {
    visible: {
      type: Boolean,
      required: true
    },
    loading: {
      type: Boolean,
      default: () => false
    },
    model: {
      type: Object,
      default: () => null
    }
  },
  data () {
    this.formLayout = {
      labelCol: {
        xs: { span: 24 },
        sm: { span: 7 }
      },
      wrapperCol: {
        xs: { span: 24 },
        sm: { span: 13 }
      }
    }
    return {
      form: this.$form.createForm(this)
    }
  },
  created () {
    console.log('custom modal created')

    // 防止表单未注册
    fields.forEach(v => this.form.getFieldDecorator(v))

    // 当 model 发生改变时，为表单设置值
    this.$watch('model', () => {
      this.model && this.form.setFieldsValue(pick(this.model, fields))
    })
  }
}
</script>
