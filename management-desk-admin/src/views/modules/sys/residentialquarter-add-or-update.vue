<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
      <el-form-item label="名称" prop="name">
        <el-input v-model="dataForm.name" placeholder="小区名称"></el-input>
      </el-form-item>
      <el-form-item label="所属社区" prop="communityCode">
        <el-input v-model="dataForm.communityCode" placeholder="所属社区编号"></el-input>
      </el-form-item>
      <el-form-item label="总栋数" prop="totalBuilding">
        <el-input v-model="dataForm.totalBuilding" placeholder="总栋数"></el-input>
      </el-form-item>
      <el-form-item label="地址" prop="address">
        <el-input v-model="dataForm.address" placeholder="地址"></el-input>
      </el-form-item>
      <el-form-item label="物业公司" prop="propertyCompany">
        <el-input v-model="dataForm.propertyCompany" placeholder="物业公司"></el-input>
      </el-form-item>
      <el-form-item label="建造时间" prop="constructionTime">
        <el-input v-model="dataForm.constructionTime" placeholder="建造时间"></el-input>
      </el-form-item>
      <el-form-item label="开发商" prop="constuctionCompany">
        <el-input v-model="dataForm.constuctionCompany" placeholder="开发商"></el-input>
      </el-form-item>
      <el-form-item label="建筑面积" prop="floorArea">
        <el-input v-model="dataForm.floorArea" placeholder="建筑面积，单位平方米"></el-input>
      </el-form-item>
      <el-form-item label="简介" prop="description">
        <el-input v-model="dataForm.description" placeholder="小区简介"></el-input>
      </el-form-item>
      <el-form-item label="所在城市" prop="cityId">
        <el-input v-model="dataForm.cityId" placeholder="所在城市"></el-input>
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
          name: '',
          communityCode: '',
          totalBuilding: '',
          address: '',
          propertyCompany: '',
          constructionTime: '',
          constuctionCompany: '',
          floorArea: '',
          description: '',
          cityId: '',
          createTime: '',
          modifyTime: ''
        },
        dataRule: {
          name: [
            { required: true, message: '小区名称不能为空', trigger: 'blur' }
          ],
          communityCode: [
            { required: true, message: '所属社区编号不能为空', trigger: 'blur' }
          ],
          address: [
            { required: true, message: '地址不能为空', trigger: 'blur' }
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
              url: this.$http.adornUrl(`/sys/residentialquarter/info/${this.dataForm.id}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.name = data.residentialquarter.name
                this.dataForm.communityCode = data.residentialquarter.communityCode
                this.dataForm.totalBuilding = data.residentialquarter.totalBuilding
                this.dataForm.address = data.residentialquarter.address
                this.dataForm.propertyCompany = data.residentialquarter.propertyCompany
                this.dataForm.constructionTime = data.residentialquarter.constructionTime
                this.dataForm.constuctionCompany = data.residentialquarter.constuctionCompany
                this.dataForm.floorArea = data.residentialquarter.floorArea
                this.dataForm.description = data.residentialquarter.description
                this.dataForm.cityId = data.residentialquarter.cityId
                this.dataForm.createTime = data.residentialquarter.createTime
                this.dataForm.modifyTime = data.residentialquarter.modifyTime
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
              url: this.$http.adornUrl(`/sys/residentialquarter/${!this.dataForm.id ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'id': this.dataForm.id || undefined,
                'name': this.dataForm.name,
                'communityCode': this.dataForm.communityCode,
                'totalBuilding': this.dataForm.totalBuilding,
                'address': this.dataForm.address,
                'propertyCompany': this.dataForm.propertyCompany,
                'constructionTime': this.dataForm.constructionTime,
                'constuctionCompany': this.dataForm.constuctionCompany,
                'floorArea': this.dataForm.floorArea,
                'description': this.dataForm.description,
                'cityId': this.dataForm.cityId,
                'createTime': this.dataForm.id === 0 ? new Date().getTime() : null,
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
