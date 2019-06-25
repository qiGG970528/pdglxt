<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
      <el-form-item label="社区编码" prop="code">
        <el-input v-model="dataForm.code" placeholder="社区编码"></el-input>
      </el-form-item>
      <el-form-item label="社区名称" prop="name">
        <el-input v-model="dataForm.name" placeholder="社区名称"></el-input>
      </el-form-item>
      <el-form-item label="所属城市" prop="cityId">
        <el-input v-model="dataForm.cityId" placeholder="所属城市代码"></el-input>
      </el-form-item>
      <el-form-item label="所属街道" prop="subdistrictCode">
        <el-input v-model="dataForm.subdistrictCode" placeholder="所属街道代码"></el-input>
      </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
  export default {
    data () {
      return {
        visible: false,
        dataForm: {
          id: 0,
          code: '',
          name: '',
          cityId: '',
          subdistrictCode: '',
          createTime: '',
          modifyTime: ''
        },
        dataRule: {
          code: [
            { required: true, message: '社区编码不能为空', trigger: 'blur' }
          ],
          name: [
            { required: true, message: '社区名称不能为空', trigger: 'blur' }
          ],
          cityId: [
            { required: true, message: '所属城市不能为空', trigger: 'blur' }
          ],
          subdistrictCode: [
            { required: true, message: '所属街道代码不能为空', trigger: 'blur' }
          ],
          createTime: [
            { required: true, message: '创建时间不能为空', trigger: 'blur' }
          ],
          modifyTime: [
            { required: true, message: '修改时间不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.id = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.id) {
            this.$http({
              url: this.$http.adornUrl(`/sys/community/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.code = data.community.code
                this.dataForm.name = data.community.name
                this.dataForm.cityId = data.community.cityId
                this.dataForm.subdistrictCode = data.community.subdistrictCode
                this.dataForm.createTime = data.community.createTime
                this.dataForm.modifyTime = data.community.modifyTime
              }
            })
          }
        })
      },
      // 表单提交
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl(`/sys/community/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'code': this.dataForm.code,
                'name': this.dataForm.name,
                'cityId': this.dataForm.cityId,
                'subdistrictCode': this.dataForm.subdistrictCode,
                'createTime': this.dataForm.id === 0 ? new Date().getTime() : undefined,
                'modifyTime': new Date().getTime()
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$message({
                  message: '操作成功',
                  type: 'success',
                  duration: 1500,
                  onClose: () => {
                    this.visible = false
                    this.$emit('refreshDataList')
                  }
                })
              } else {
                this.$message.error(data.msg)
              }
            })
          }
        })
      }
    }
  }
</script>
