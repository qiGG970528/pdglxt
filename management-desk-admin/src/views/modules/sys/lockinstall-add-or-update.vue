<template>
  <el-dialog :title="!dataForm.id ? '新增' : '修改'" :close-on-click-modal="false" :visible.sync="visible">
    <div class="twoOne" v-show="!dataForm.id">(详细地址中  小区和街道二选一)</div>
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
      <el-form-item label="标题" prop="title">
        <el-input v-model="dataForm.title" placeholder="标题"></el-input>
      </el-form-item>
      <el-form-item label="所在地区" prop="cityValue">
        <el-select v-model="dataForm.cityValue" clearable placeholder="请选择市" style="width:180px">
          <el-option value="3301" label="杭州市">杭州市</el-option>
        </el-select>
      </el-form-item>
      <el-form-item prop="areaValue" label-width="0">
        <el-select v-model="dataForm.areaValue" clearable placeholder="请选择区" @change="choseArea" style="width:180px">
          <el-option v-for="item in area" :key="item.value" :label="item.label" :value="item.value">
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item label-width="0" prop="streetValue">
        <el-select v-model="dataForm.streetValue" clearable placeholder="请选择街道" @change="choseStreet" style="width:180px">
          <el-option v-for="item in street" :key="item.code" :label="item.name" :value="item.code">
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="所属社区" prop="communityCode">
        <el-select v-model="dataForm.communityCode" clearable placeholder="请选择社区" @change="choseCommunity" style="width:180px">
          <el-option v-for="item in community" :key="item.code" :label="item.name" :value="item.code">
          </el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="详细地址">
          <el-switch
            v-model="detailA"
            active-text="按小区添"
            inactive-text="按街道添"
            @change='changeStatus'>
          </el-switch>
      </el-form-item>
      <div v-if="detailA">
          <el-form-item label="小区" prop="housingEstateValue" label-width="90px">
            <el-select v-model="dataForm.housingEstateValue" clearable placeholder="请选择小区" style="width:180px">
              <el-option v-for="item in housingEstate" :key="item.id" :label="item.name" :value="item.id">
              </el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="幢" prop="buildingNumber" label-width="90px">
            <el-input v-model="dataForm.buildingNumber" style="width: 5rem" placeholder="幢"></el-input>
          </el-form-item>
          <el-form-item label="单元" prop="unitNumber" label-width="90px">
            <el-input v-model="dataForm.unitNumber" style="width: 5rem" placeholder="单元"></el-input>
          </el-form-item>
          <el-form-item label="门牌号" prop="roomNumber" label-width="90px">
            <el-input v-model="dataForm.roomNumber" style="width: 5rem" placeholder="房间"></el-input>
          </el-form-item>
      </div>
      <div v-if="!detailA">
        <el-form-item label="街道" prop="detailedAddress" label-width="90px">
          <el-input v-model="dataForm.streetNumber"  placeholder="街道"></el-input>
        </el-form-item>
        <el-form-item label="门牌号" prop="detailedAddress" label-width="90px">
          <el-input v-model="dataForm.doorNumber" style="width: 5rem" placeholder="门牌号"></el-input>
        </el-form-item>
      </div>
      <el-form-item label="租金" prop="rent">
        <el-input type="number" v-model="dataForm.rent" placeholder="租金"></el-input>
      </el-form-item>
      <el-form-item label="面积(㎡)" prop="proportion">
        <el-input type="number" v-model="dataForm.proportion" placeholder="面积（㎡）"></el-input>
      </el-form-item>
      <el-form-item label="电梯" prop="elevator" label-width="90px">
        <el-select v-model="dataForm.elevator" placeholder="请选择电梯" style="width:180px">
          <el-option value="0" label="无"></el-option>
          <el-option value="1" label="有"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="户型" prop="bedrooms" style="display:inline-block;">
        <el-input type="number" v-model="dataForm.bedrooms" placeholder="室"></el-input>
        <span>室</span>
      </el-form-item>
      <el-form-item prop="livingrooms" style="display:inline-block;" label-width="0">
        <el-input type="number" v-model="dataForm.livingrooms" placeholder="厅"></el-input>
        <span>厅</span>
      </el-form-item>
      <el-form-item label="厨房" prop="kitchenType" style="display:inline-block;">
        <el-select v-model="dataForm.kitchenType" placeholder="请选择厨房类型" style="width:160px">
          <el-option value="0" label="无"></el-option>
          <el-option value="1" label="独立"></el-option>
          <el-option value="2" label="公用"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item prop="kitchenrooms" style="display:inline-block;" label-width="0">
        <el-input type="number" v-model="dataForm.kitchenrooms" placeholder="个" style="width:160px"></el-input>
        <span>个</span>
      </el-form-item>
      <el-form-item label="卫生间" prop="bathroomType" style="display:inline-block;">
        <el-select v-model="dataForm.bathroomType" placeholder="请选择卫生间类型">
          <el-option value="1" label="独立"></el-option>
          <el-option value="2" label="公用"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item prop="bathrooms" style="display:inline-block;" label-width="0">
        <el-input type="number" v-model="dataForm.bathrooms" placeholder="个"></el-input>
        <span>个</span>
      </el-form-item>

      <el-form-item label="装修" prop="decoration">
        <el-select v-model="dataForm.decoration" placeholder="请选择装修类型">
          <el-option value="1" label="毛坯"></el-option>
          <el-option value="2" label="简装"></el-option>
          <el-option value="3" label="精装"></el-option>
          <el-option value="4" label="豪华"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="朝向" prop="towards">
        <el-select v-model="dataForm.towards" placeholder="请选择朝向" style="width:180px">
          <el-option value="1" label="南北"></el-option>
          <el-option value="2" label="南"></el-option>
          <el-option value="3" label="东"></el-option>
          <el-option value="4" label="西"></el-option>
          <el-option value="5" label="北"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="所在楼层" prop="floorNumber">
        <el-input type="number" v-model="dataForm.floorNumber" placeholder="所在楼层"></el-input>
      </el-form-item>
      <el-form-item label="总楼层" prop="totalFloor">
        <el-input type="number" v-model="dataForm.totalFloor" placeholder="总楼层"></el-input>
      </el-form-item>
      <el-form-item label="建造年代" prop="constructionTime">
        <el-select v-model="dataForm.constructionTime" placeholder="请选择建造年代" style="width:180px">
          <el-option value="2018" label="2018"></el-option>
          <el-option value="2017" label="2017"></el-option>
          <el-option value="2016" label="2016"></el-option>
          <el-option value="2015" label="2015"></el-option>
          <el-option value="2014" label="2014"></el-option>
          <el-option value="2013" label="2013"></el-option>
          <el-option value="2012" label="2012"></el-option>
          <el-option value="2011" label="2011"></el-option>
          <el-option value="2010" label="2010"></el-option>
          <el-option value="2009" label="2009"></el-option>
          <el-option value="2008" label="2008"></el-option>
          <el-option value="2007" label="2007"></el-option>
          <el-option value="2006" label="2006"></el-option>
          <el-option value="2005" label="2005"></el-option>
          <el-option value="2004" label="2004"></el-option>
          <el-option value="2003" label="2003"></el-option>
          <el-option value="2002" label="2002"></el-option>
          <el-option value="2001" label="2001"></el-option>
          <el-option value="2000" label="2000"></el-option>
          <el-option value="1999" label="1999"></el-option>
          <el-option value="1998" label="1998"></el-option>
          <el-option value="1997" label="1997"></el-option>
          <el-option value="1996" label="1996"></el-option>
          <el-option value="1995" label="1995"></el-option>
          <el-option value="1994" label="1994"></el-option>
          <el-option value="1993" label="1993"></el-option>
          <el-option value="1992" label="1992"></el-option>
          <el-option value="1991" label="1991"></el-option>
          <el-option value="1990" label="1990"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="支付方式" prop="payType">
        <el-select v-model="dataForm.payType" placeholder="请选择支付方式" style="width:180px">
          <el-option value="0" label="无限制"></el-option>
          <el-option value="1" label="月付"></el-option>
          <el-option value="2" label="季付"></el-option>
          <el-option value="3" label="年付"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="出租方式" prop="payType">
        <el-select v-model="dataForm.payType" placeholder="请选择出租方式" style="width:180px">
          <el-option value="1" label="整租"></el-option>
          <el-option value="2" label="合租"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="简介" prop="intro">
        <el-input v-model="dataForm.intro" placeholder="简介"></el-input>
      </el-form-item>
      <!-- 房屋配置 -->
      <el-form-item label="房屋配置" prop="furnitureCheckList">
        <el-checkbox-group v-model="furnitureCheckList" style="display:inline-block">
          <el-checkbox v-for="item in dataForm.furniture" :label="item.label" :key="item.label">{{item.value}}</el-checkbox>
          <!-- <el-checkbox label="10021">床</el-checkbox>
          <el-checkbox label="10022">衣柜</el-checkbox>
          <el-checkbox label="10023">书桌</el-checkbox>
          <el-checkbox label="10024">椅子</el-checkbox>
          <el-checkbox label="10025">床头柜</el-checkbox> -->
        </el-checkbox-group>
        <el-checkbox-group v-model="electricCheckList" style="display:inline-block" @change="checked">
          <el-checkbox label="10011" name="空调">空调</el-checkbox>
          <el-checkbox label="10012">洗衣机</el-checkbox>
          <el-checkbox label="10013">冰箱</el-checkbox>
          <el-checkbox label="10014">电视</el-checkbox>
          <el-checkbox label="10015">热水器</el-checkbox>
          <el-checkbox label="10016">网络</el-checkbox>
          <el-checkbox label="10017">天然气</el-checkbox>
        </el-checkbox-group>
      </el-form-item>
      <!-- 房屋图片 -->
      <el-form-item label="房屋图片">
        <el-upload action="http://www.iyuebanwan.com/easyrent/pub/uploadoss" list-type="picture-card" :on-preview="handlePictureCardPreview" :on-remove="handleRemove" :on-success="handleSuccess" :limit="9" :on-exceed="handleExceed" :before-upload="beforeAvatarUpload">
          <i class="el-icon-plus"></i>
        </el-upload>
        <el-dialog :visible.sync="dialogVisible">
          <img width="100%" :src="dialogImageUrl" alt="">
        </el-dialog>
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
  data() {
    return {
      detailA: true,
      visible: false,
      furnitureCheckList: [],
      electricCheckList: [],
      pictures: [],
      street: [], //街道
      community: [], //社区
      housingEstate: [], //小区
      dialogImageUrl: "",
      dialogVisible: false,
      dataForm: {
        title: "", //标题
        cityValue: "", //市
        areaValue: "", //区
        streetValue: "", //街道
        communityCode: "", //社区
        housingEstateValue: "", //小区
        detailedAddress: "", //详细地址
        buildingNumber: '',//幢
        unitNumber: '',//单元
        roomNumber: '',//房间
        streetNumber: '',//街道
        doorNumber: '',//门牌号
        id: 0,
        rent: "",
        proportion: "",
        bedrooms: "",
        livingrooms: "",
        kitchenType: "",
        kitchenrooms: "",
        bathroomType: "",
        bathrooms: "",
        elevator: "",
        decoration: "",
        towards: "",
        floorNumber: "",
        totalFloor: "",
        constructionTime: "",
        payType: "",
        rentType: "",
        intro: "",
        furniture: [
          { label: 10021, value: "床" },
          { label: 10022, value: "衣柜" },
          { label: 10023, value: "书桌" },
          { label: 10024, value: "椅子" },
          { label: 10025, value: "床头柜" }
        ]
      },
      area: [
        { value: 330102, label: "上城区" },
        { value: 330103, label: "下城区" },
        { value: 330104, label: "江干区" },
        { value: 330105, label: "拱墅区" },
        { value: 330106, label: "西湖区" },
        { value: 330108, label: "滨江区" },
        { value: 330109, label: "萧山区" },
        { value: 330110, label: "余杭区" },
        { value: 330111, label: "富阳区" },
        { value: 330112, label: "临安区" },
        { value: 330122, label: "桐庐县" },
        { value: 330127, label: "淳安县" },
        { value: 330182, label: "建德市" }
      ],
      dataRule: {
        title: [{ required: true, message: "标题不能为空", trigger: "blur" }],
        cityValue: [
          { required: true, message: "所在城市不能为空", trigger: "blur" }
        ],
        areaValue: [
          { required: true, message: "所属区域不能为空", trigger: "blur" }
        ],
        streetValue: [
          { required: true, message: "所属街道不能为空", trigger: "blur" }
        ],
        communityCode: [
          { required: true, message: "所属社区不能为空", trigger: "blur" }
        ],
        housingEstateValue: [
          { required: true, message: "所属小区不能为空", trigger: "blur" }
        ],
        detailedAddress: [
          { required: true, message: "地址不能为空", trigger: "blur" }
        ],
        buildingNumber: [
          { required: true, message: "请填写幢", trigger: "blur" }
        ],
        unitNumber: [
          { required: true, message: "请填写单元", trigger: "blur" }
        ],
        roomNumber: [
          { required: true, message: "请填写房间号", trigger: "blur" }
        ],
      }
    };
  },
  methods: {
    //详细地址安小区添还是街道
    changeStatus: function(callback){
      this.detailA = callback
    },
    checked() {
      console.log(this);
    },
    //获取街道代码
    choseArea() {
      var _this = this;
      this.$http({
        url: this.$http.adornUrl(`/sys/subdistrict/list`),
        method: "get",
        params: this.$http.adornParams({
          code: this.dataForm.areaValue
        })
      })
        .then(data => {
          if (data.data.resultCode == 1000) {
            this.street = data.data.page.list;
          }
        })
        .catch(function(err) {
          console.log("失败");
        });
    },
    //获取社区编码
    choseStreet() {
      var _this = this;
      this.$http({
        url: this.$http.adornUrl(`/sys/community/list`),
        method: "get",
        params: this.$http.adornParams({
          code: this.dataForm.streetValue
        })
      })
        .then(data => {
          if (data.data.resultCode == 1000) {
            this.community = data.data.page.list;
          }
        })
        .catch(function(err) {
          console.log("失败");
        });
    },
    //获取小区编码
    choseCommunity() {
      var _this = this;
      this.$http({
        url: this.$http.adornUrl(`/sys/residentialquarter/list`),
        method: "get",
        params: this.$http.adornParams({
          code: this.dataForm.communityCode
        })
      })
        .then(data => {
          if (data.data.resultCode == 1000) {
            this.housingEstate = data.data.page.list;
          }
        })
        .catch(function(err) {
          console.log("失败");
        });
    },
    //文件列表移除文件时的钩子
    handleRemove(file, fileList) {
      console.log(file, fileList);
    },
    //点击文件列表中已上传的文件时的钩子
    handlePictureCardPreview(file) {
      this.dialogImageUrl = file.url;
      this.dialogVisible = true;
    },
    //限制文件大小
    beforeAvatarUpload(file) {
      const isLt2M = file.size / 1024 / 1024 < 1;
      if (!isLt2M) {
        this.$message.error("上传头像图片大小不能超过 1MB!");
      }
      return isLt2M;
    },
    //文件超出个数时限制的钩子
    handleExceed(file, fileList) {
      this.$message({
        message: "最多只能上传九张哦！",
        type: "warning"
      });
    },
    //图片上传成功处理
    handleSuccess(response, file, fileList) {
      if (response.resultCode == 1000) {
        for (var i = 0; i < response.resultData.length; i++) {
          var a = { key: response.resultData[i].title };
          this.pictures.push(a);
        }
      } else {
        this.$message.error("上传失败");
      }
    },

    init(id) {
      this.dataForm.id = id || 0;
      this.visible = true;
      this.$nextTick(() => {
        this.$refs["dataForm"].resetFields();
        if (this.dataForm.id) {
          this.$http({
            url: this.$http.adornUrl(
              `/sys/lockinstall/info/${this.dataForm.id}`
            ),
            method: "get",
            params: this.$http.adornParams()
          }).then(({ data }) => {
            if (data && data.code === 0) {
              this.dataForm.rent = data.lockinstall.rent;
              this.dataForm.proportion = data.lockinstall.proportion;
              this.dataForm.communityCode = data.lockinstall.communityCode;
              this.dataForm.address = data.lockinstall.address;
              this.dataForm.doorLock = data.lockinstall.doorLock;
              this.dataForm.hallwayLock = data.lockinstall.hallwayLock;
              this.dataForm.applyUid = data.lockinstall.applyUid;
              this.dataForm.applyTime = data.lockinstall.applyTime;
              this.dataForm.locksmithSysUid = data.lockinstall.locksmithSysUid;
              this.dataForm.auditUid = data.lockinstall.auditUid;
              this.dataForm.auditTime = data.lockinstall.auditTime;
              this.dataForm.step = data.lockinstall.step;
              this.dataForm.status = data.lockinstall.status;
              this.dataForm.remark = data.lockinstall.remark;
              this.dataForm.failMessage = data.lockinstall.failMessage;
              this.dataForm.createTime = data.lockinstall.createTime;
              this.dataForm.modifyTime = data.lockinstall.modifyTime;
            }
          });
        }
      });
    },
    // 表单提交
    dataFormSubmit() {
      const that = this;
      const userId = that.$cookie.get('userId'); 
      var data = {
        title: this.dataForm.title,
        communityCode: this.dataForm.communityCode,
        rqId: this.dataForm.housingEstateValue,
        buildingNumber: this.dataForm.buildingNumber,
        unitNumber: this.dataForm.unitNumber,
        roomNumber: this.dataForm.roomNumber,
        createUid: userId
      };
      console.log(data);
      this.$refs["dataForm"].validate(valid => {
        if (valid) {
          this.axios({
            url: "audit/updatedetail",
            method: "post",
            data: data
          })
            .then(data => {
              if (data.data.resultCode == 1000) {
                console.log(data.data.resultData)
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
            .catch(function(err) {
              console.log(err);
              this.dataListLoading = false;
            });
        }
      });
      // this.$refs['dataForm'].validate((valid) => {
      //   if (valid) {
      //     this.$http({
      //       url: this.$http.adornUrl(`/audit/updatedetail/${!this.dataForm.id ? 'save' : 'update'}`),
      //       method: 'post',
      //       headers:{'Access-Control-Allow-Origin': '*'},
      //       data: this.$http.adornData({
      //         'id': this.dataForm.id || undefined,
      //         'rent': this.dataForm.rent,
      //         'proportion': this.dataForm.proportion,
      //         'communityCode': this.dataForm.communityCode,
      //         'address': this.dataForm.address,
      //         'doorLock': this.dataForm.doorLock,
      //         'hallwayLock': this.dataForm.hallwayLock,
      //         'applyUid': this.dataForm.applyUid,
      //         'applyTime': this.dataForm.applyTime,
      //         'auditUid': this.dataForm.auditUid,
      //         'auditTime': this.dataForm.auditTime,
      //         'status': this.dataForm.status,
      //         'remark': this.dataForm.remark,
      //         'failMessage': this.dataForm.failMessage,
      //         'createTime': this.dataForm.createTime,
      //         'modifyTime': this.dataForm.modifyTime
      //       })
      //     }).then(({data}) => {
      //       if (data && data.code === 0) {
      //         this.$message({
      //           message: '操作成功',
      //           type: 'success',
      //           duration: 1500,
      //           onClose: () => {
      //             this.visible = false
      //             this.$emit('refreshDataList')
      //           }
      //         })
      //       } else {
      //         this.$message.error(data.msg)
      //       }
      //     })
      //   }
      // })
    }
  }
};
</script>
<style scoped>
.el-checkbox + .el-checkbox {
  margin-left: 16px;
}
.el-upload-list--picture-card .el-upload-list__item {
  width: 75px;
  height: 75px;
}
.el-upload--picture-card {
  width: 75px;
  height: 75px;
  line-height: 85px;
}
.twoOne {
  text-align: center;
    margin-bottom: 10px;
}
/* .el-select .el-input{
    width: 180px;
  } */
.el-input {
  width: 180px;
}
.el-form-item {
  display: inline-block;
}
/* .el-select{
    width: 180px;
  } */
</style>

