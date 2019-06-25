<template>
    <div class="Xq1">
        <el-main>
            <el-row style="width: 80%;display: inline-block;">
                <el-col :span="8">
                    <div class="grid-content bg-purple">房东：王力</div>
                </el-col>
                <el-col :span="8">
                    <div class="grid-content bg-purple-light">房东电话：13453738166</div>
                </el-col>
                <el-col :span="8"><div class="grid-content bg-purple">备案时间：2018.0810</div></el-col>
            </el-row>
            <el-row style="width: 80%;display: inline-block;">
                <el-col :span="8">
                    <div class="grid-content bg-purple">预审员：张晓</div>
                </el-col>
                <el-col :span="8">
                    <div class="grid-content bg-purple-light">联系电话：13453738166</div>
                </el-col>
                <el-col :span="8"><div class="grid-content bg-purple">预审时间：2018.08.11</div></el-col>
            </el-row>
            <el-row style="width: 80%;display: inline-block;">
                <el-col :span="8">
                    <div class="grid-content bg-purple">所在地区：上城区望江街道徐家汇社区</div>
                </el-col>
                <el-col :span="8">
                    <div class="grid-content bg-purple-light">房屋地址：婺江家园10幢3单元201室</div>
                </el-col>
                <el-col :span="8"><div class="grid-content bg-purple" style="color: #ff3333">得分：86</div></el-col>
            </el-row>
            <div class="Div4">
                <el-card class="box-card">
                    <div slot="header" class="clearfix">
                        <span>消防安全</span>
                    </div>
                    <el-checkbox-group v-model="checkedCities3">
                        <el-row>
                            <el-col :span="12"><el-checkbox label="消防四件套" name="type"></el-checkbox></el-col>
                            <el-col :span="12"><el-checkbox label="室内无易燃物" name="type"></el-checkbox></el-col>
                        </el-row>
                        <el-row>
                            <el-col :span="12"><el-checkbox label="智能烟感设备" name="type"></el-checkbox></el-col>
                            <el-col :span="12"><el-checkbox label="燃气符合规范" name="type"></el-checkbox></el-col>
                        </el-row>
                        <el-row>
                            <el-col :span="12"><el-checkbox label="电路安全，安装电闸，无私拉乱接" name="type"></el-checkbox></el-col>
                            <el-col :span="12"><el-checkbox label="电瓶车集中放置，不在安全出口、疏散通道处充电" name="type"></el-checkbox></el-col>
                        </el-row>
                        <el-row>
                            <el-col :span="12"><el-checkbox label="疏散逃生通道、楼梯间畅通" name="type"></el-checkbox></el-col>
                            <el-col :span="12"><el-checkbox label="室内电器产品符合相关安全标准" name="type"></el-checkbox></el-col>
                        </el-row>
                    </el-checkbox-group>
                    <el-row>
                        <el-col :span="24"><div class="grid-content bg-purple-dark">现场照片：</div></el-col>
                    </el-row>
                </el-card>
                <el-card class="box-card">
                    <div slot="header" class="clearfix">
                        <span>治安安全</span>
                    </div>
                    <el-checkbox-group v-model="checkedCities2">
                        <el-row>
                            <el-col :span="12"><el-checkbox label="单元安装视屏监控、智能门禁" name="type"></el-checkbox></el-col>
                            <el-col :span="12"><el-checkbox label="房屋安装智能门锁" name="type"></el-checkbox></el-col>
                        </el-row>
                        <el-row>
                            <el-col :span="12"><el-checkbox label="出租房屋基本信息报送" name="type"></el-checkbox></el-col>
                            <el-col :span="12"><el-checkbox label="入住人员信息登记" name="type"></el-checkbox></el-col>
                        </el-row>
                    </el-checkbox-group>
                    <el-row>
                        <el-col :span="24"><div class="grid-content bg-purple-dark">现场照片：</div></el-col>
                    </el-row>
                </el-card>

                <el-card class="box-card">
                    <div slot="header" class="clearfix">
                        <span>建筑安全</span>
                    </div>
                    <el-checkbox-group
                    v-model="checkedCities1">
                        <el-row>
                            <el-col :span="12"><el-checkbox label="无违章搭建、分割房屋" name="type"></el-checkbox></el-col>
                            <el-col :span="12"><el-checkbox label="房屋所在楼房不是危房" name="type"></el-checkbox></el-col>
                        </el-row>
                        <el-row>
                            <el-col :span="12"><el-checkbox label="居室人均使用面积不低于4平米" name="type"></el-checkbox></el-col>
                            <el-col :span="12"><el-checkbox label="居室居住人数没有超过2人（有法定赡养、抚养义务的除外）" name="type"></el-checkbox></el-col>
                        </el-row>
                        <el-row>
                            <el-col :span="12"><el-checkbox label="没有改变房屋使用性质" name="type"></el-checkbox></el-col>
                            <el-col :span="12"><el-checkbox label="出租房屋不是餐厅、厨房、卫生间、阳台、地下室等非居住空间" name="type"></el-checkbox></el-col>
                        </el-row>
                    </el-checkbox-group>
                    <el-row>
                        <el-col :span="24"><div class="grid-content bg-purple-dark">现场照片：</div></el-col>
                    </el-row>
                </el-card>
                <!-- 房源介绍 -->
                <div class="fouurDiv">
                    <el-form label-width="80px">
                        <el-form-item label="评定建议">
                            <el-input type="textarea"></el-input>
                        </el-form-item>
                        <el-form-item>
                            <el-button type="primary" @click="onSubmit">通过</el-button>
                            <el-button>驳回    </el-button>
                        </el-form-item>
                    </el-form>
                </div>
            </div>   
        </el-main>
        <!-- 新增房东信息 -->
        <el-dialog title="房东信息" :close-on-click-modal="false" :visible.sync="dialogFormVisible" center>
            <el-form class="Form" :rules="rules" :model="addPersonForm" ref="addPersonForm">
                <el-form-item label="姓名" prop="name">
                    <el-input v-model="addPersonForm.name"></el-input>
                </el-form-item>
                <el-form-item label="手机号" prop="phone">
                    <el-input v-model="addPersonForm.phone"></el-input>
                </el-form-item>
                <el-form-item label="身份证" prop="IdCard">
                    <el-input v-model="addPersonForm.IdCard"></el-input>
                </el-form-item>
                <el-form-item label="备注">
                    <el-input v-model="addPersonForm.beiZ"></el-input>
                </el-form-item>
                <el-form-item style="text-align: left;" label="身份证正、反面照片" required>
                    <el-upload action="http://www.iyuebanwan.com/easyrent/pub/uploadoss" list-type="picture-card" :on-preview="handlePictureCardPreview"
                        :on-remove="handleRemove" :on-success="handleSuccess" :limit="2" :on-exceed="handleExceed" :before-upload="beforeAvatarUpload">
                        <i class="el-icon-plus"></i>
                    </el-upload>
                    <el-dialog :visible.sync="dialogVisible">
                        <img width="100%" :src="dialogImageUrl" alt="">
                    </el-dialog>
                </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button @click="dialogFormVisible = false">取 消</el-button>
                <el-button type="primary" @click="submitForm('addPersonForm')">确 定</el-button>
            </div>
        </el-dialog>
        <!-- 编辑产权信息 -->
        <el-dialog title="产权信息" :close-on-click-modal="false" :visible.sync="dialogCq" center>
            <el-form class="Form" :model="cqForm" >
                <el-form-item style="text-align: left;" label="产权图片" required>
                    <el-upload action="http://www.iyuebanwan.com/easyrent/pub/uploadoss" list-type="picture-card" :on-preview="handleCqPreview"
                        :on-remove="cqRemove" :on-success="cqSuccess" :limit="1" :on-exceed="cqExceed" :before-upload="beforeCqUpload">
                        <i class="el-icon-plus"></i>
                    </el-upload>
                    <el-dialog :visible.sync="dialogVisibleCq">
                        <img width="100%" :src="dialogCqUrl" alt="">
                    </el-dialog>
                </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button @click="dialogCq = false">取 消</el-button>
                <el-button type="primary" @click="submitCq('cqForm')">确 定</el-button>
            </div>
        </el-dialog>
    </div>
</template>
<script>
    export default {
        data() {
            return {
                dialogCq: false,
                cqImg: '',
                cqTp: '',
                dialogFormVisible: false,
                dialogVisible: false,
                dialogVisibleCq: false,
                dialogCq: false,
                fdModel: true,
                dialogImageUrl: "",
                dialogCqUrl: '',
                houseId: "",
                houseXQdata: "",
                checkedCities1: ['无违章搭建、分割房屋', '出租房屋不是餐厅、厨房、卫生间、阳台、地下室等非居住空间'],
                checkedCities2: ['入住人员信息登记','房屋安装智能门锁'],
                checkedCities3: ['消防四件套','室内无易燃物','智能烟感设备','疏散逃生通道、楼梯间畅通','电瓶车集中放置，不在安全出口、疏散通道处充电'],
                URL: "https://gou.goucaiyun.net/",
                // 房屋配置复选
                electric: [],//家电
                furniture: [],//家具
                pictures: [],
                KTchecked: false,
                xyjchecked: false,
                bxchecked: false,
                dianshijichecked: false,
                rsqchecked: false,
                kuandaichecked: false,
                trqchecked: false,
                chuangchecked: false,
                yiguichecked: false,
                shuzhuochecked: false,
                yizichecked: false,
                ctgchecked: false,
                landlords: [],//房东列表
                renters: [],//房客列表
                addPersonForm: {
                    name: "",
                    phone: "",
                    IdCard: "",
                    beiZ: "",
                    community: "",
                    area: "",
                    id: ''
                },
                cqForm: {
                    CqImg_r: ''
                },
                IdCardImg: '',
                house: {
                    cq: ''
                },
                rules: {
                    name: [
                        { required: true, message: "请输入姓名", trigger: "blur" },
                        { min: 2, max: 5, message: "长度在 2 到 5 个字符", trigger: "blur" }
                    ],
                    phone: [{ required: true, message: "请输入手机号", trigger: "blur" },
                    { pattern: /^1[34578]\d{9}$/, message: '手机号码格式不正确', trigger: "blur" }],
                    IdCard: [
                        { required: true, message: "请输入身份证", trigger: "blur" },
                        { pattern: /^\d{6}(18|19|20)?\d{2}(0[1-9]|1[012])(0[1-9]|[12]\d|3[01])\d{3}(\d|[xX])$/, message: '身份证格式不正确', trigger: "blur" }
                    ]
                }
            }
        },
        activated() {
            this.houseId = this.$cookie.get('houseId');
            this.houseXQ();
            this.preon();
            this.viewdata();
        },
        methods: {
            houseXQ() {
                var that = this;
                var data = {
                    "id": parseInt(this.houseId)
                }
                this.axios({
                    url: "house/detail",
                    method: 'post',
                    data: data
                })
                    .then((data) => {
                        if (data.data.resultCode == 1000) {
                            this.houseXQdata = data.data.resultData;
                            this.departTypeTitle = data.data.resultData.departType.title;
                            this.decorationTitle = data.data.resultData.decoration.title;
                            this.towardsTitle = data.data.resultData.towards.title;
                            this.decorationTitle = data.data.resultData.decoration.title;
                            this.kitchenTypeTitle = data.data.resultData.kitchenType.title;
                            this.bathroomTypeTitle = data.data.resultData.bathroomType.title;
                            this.elevatorTitle = data.data.resultData.elevator.title;
                            this.housePictures = data.data.resultData.pictures;
                            this.electric = data.data.resultData.electric;//家电
                            this.furniture = data.data.resultData.furniture;//家具
                            for (var i = 0; i < this.electric.length; i++) {//家电
                                if (this.electric[i].value == 10011) {
                                    this.KTchecked = true;
                                } else if (this.electric[i].value == 10012) {
                                    this.xyjchecked = true;
                                } else if (this.electric[i].value == 10013) {
                                    this.bxchecked = true;
                                } else if (this.electric[i].value == 10014) {
                                    this.dianshijichecked = true;
                                } else if (this.electric[i].value == 10015) {
                                    this.rsqchecked = true;
                                } else if (this.electric[i].value == 10016) {
                                    this.kuandaichecked = true;
                                } else if (this.electric[i].value == 10017) {
                                    this.trqchecked = true;
                                }
                            }
                            for (var i = 0; i < this.furniture.length; i++) {//家具
                                if (this.furniture[i].value == 10021) {
                                    this.chuangchecked = true;
                                } else if (this.furniture[i].value == 10022) {
                                    this.yiguichecked = true;
                                } else if (this.furniture[i].value == 10023) {
                                    this.shuzhuochecked = true;
                                } else if (this.furniture[i].value == 10024) {
                                    this.yizichecked = true;
                                } else if (this.furniture[i].value == 10025) {
                                    this.ctgchecked = true;
                                }
                            }
                        }
                    })
                    .catch(function (err) {
                        console.log("失败")
                    })
            },
            //获取房屋内人员（房东和租客）
            preon() {
                this.axios({
                    url: "person/querybyapartmentid",
                    method: "post",
                    data: { apartmentId: this.houseId }
                })
                    .then(data => {
                        if (data.data.resultCode == 1000) {
                            if (data.data.resultData.landlords.length != 0) {
                                this.addPersonForm.name = data.data.resultData.landlords[0].name;
                                this.addPersonForm.phone = data.data.resultData.landlords[0].phone;
                                this.addPersonForm.IdCard = data.data.resultData.landlords[0].identityCard;
                                this.addPersonForm.id = data.data.resultData.landlords[0].id;
                                this.fdModel = true;
                            } else {
                                this.fdModel = false;
                            }
                        }
                    })
                    .catch(function (err) {
                        console.log(err);
                    });
            },
            //获取房屋消防和产权
            viewdata() {
                this.axios({
                    url: "audit/viewdata",
                    method: "post",
                    data: { id: this.houseId }
                })
                .then(data => {
                    if (data.data.resultCode == 1000) {
                        console.log(data)
                    }
                })
                .catch(function (err) {
                    console.log(err);
                });
            },
            //身份证文件列表移除文件时的钩子
            handleRemove(file, fileList) {
                console.log(file, fileList);
            },
            //身份证点击文件列表中已上传的文件时的钩子
            handlePictureCardPreview(file) {
                this.dialogImageUrl = file.url;
                this.dialogVisible = true;
            },
            //身份证限制文件大小
            beforeAvatarUpload(file) {
                const isLt2M = file.size / 1024 / 1024 < 1;
                if (!isLt2M) {
                    this.$message.error("上传头像图片大小不能超过 1MB!");
                }
                return isLt2M;
            },
            //身份证文件超出个数时限制的钩子
            handleExceed(file, fileList) {
                this.$message({
                    message: "最多只能上传2张哦！",
                    type: "warning"
                });
            },   
            //身份证图片上传成功处理
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
            //产权文件列表移除文件时的钩子
            cqRemove(file, fileList) {
                console.log(file, fileList);
            },
            //产权点击文件列表中已上传的文件时的钩子
            handleCqPreview(file) {
                this.dialogCqUrl = file.url;
                this.dialogVisibleCq = true;
            },
            //产权限制文件大小
            beforeCqUpload(file) {
                const isLt2M = file.size / 1024 / 1024 < 1;
                if (!isLt2M) {
                    this.$message.error("上传头像图片大小不能超过 1MB!");
                }
                return isLt2M;
            },
            //产权文件超出个数时限制的钩子
            cqExceed(file, fileList) {
                this.$message({
                    message: "最多只能上传1张哦！",
                    type: "warning"
                });
            },
            //产权图片上传成功处理
            cqSuccess(response, file, fileList) {
                if (response.resultCode == 1000) {
                    for (var i = 0; i < response.resultData.length; i++) {
                        var a = { key: response.resultData[i].title };
                        this.cqImg = this.URL + a.key;
                    }
                } else {
                    this.$message.error("上传失败");
                }
            },
            //提交房东信息
            submitForm(formName) {
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        if(this.pictures.length == 2) {
                            const url = this.fdModel ? 'audit/landlord/update' : 'audit/landlord/save';
                            const data = this.fdModel ? {
                                apartmentId: parseInt(this.houseId),
                                name: this.addPersonForm.name,
                                phone: this.addPersonForm.phone,
                                idCard: this.addPersonForm.IdCard,
                                intro: this.addPersonForm.beiZ,
                                id: this.addPersonForm.id,
                                idCardFrontPic: this.URL + this.pictures[0].key,
                                idCardBackPic: this.URL + this.pictures[1].key,
                            } : {
                                    apartmentId: parseInt(this.houseId),
                                    name: this.addPersonForm.name,
                                    phone: this.addPersonForm.phone,
                                    idCard: this.addPersonForm.IdCard,
                                    intro: this.addPersonForm.beiZ,
                                    idCardFrontPic: this.URL + this.pictures[0].key,
                                    idCardBackPic: this.URL + this.pictures[1].key,
                                }
                            this.axios({
                                url: "audit/landlord/save",
                                method: "post",
                                data: data
                            })
                            .then(data => {
                                if (data.data.resultCode == 1000) {
                                    this.addPersonForm.name = data.data.resultData.name;
                                    this.addPersonForm.phone = data.data.resultData.phone;
                                    this.addPersonForm.IdCard = data.data.resultData.idCard;
                                    this.addPersonForm.beiZ = data.data.resultData.intro;
                                    this.fdModel = true;
                                    this.$message({
                                        message: '操作成功',
                                        type: 'success',
                                        duration: 1500,
                                        onClose: () => {
                                            this.dialogFormVisible = false
                                        }
                                    })
                                }
                            })
                            .catch(function (err) {
                                console.log(err);
                            });
                        } else {
                            this.$message({
                            message: '请上传身份证正反面图片！',
                            type: 'warning'
                            });
                        }
                    } else {
                        console.log('error submit!!');
                        return false;
                    }
                });
            },
            //提交产权图片
            submitCq(formName) {
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        if(this.cqImg) {
                            this.axios({
                                url: "audit/uploaddata",
                                method: "post",
                                data: { 
                                    "apartmentId": this.houseId,
                                    "pictures": [
                                        {
                                            "url": this.cqImg,
                                            "type": 0
                                        }
                                    ] 
                                }
                            })
                            .then(data => {
                                if (data.data.resultCode == 1000) {
                                    this.cqTp = this.cqImg;
                                    this.$message({
                                        message: '产权信息提交成功',
                                        type: 'success',
                                        duration: 1500,
                                        onClose: () => {
                                            this.dialogCq = false
                                        }
                                    })
                                }
                            })
                            .catch(function (err) {
                                console.log(err);
                            })
                        } else {
                            this.$message({
                            message: '请上传产权图片！',
                            type: 'warning'
                            });
                        }
                    }
                })
                
            }
        }
    }
</script>
<style scoped>
    .Xq1 {
        margin-top: 1.25rem;
        font-size: 16px;
    }

    .el-main {
        display: inline-block;
        height: 100%;
        background-color: #fff;
        width: 100%;
        padding: 58px 0 100px 0;
        line-height: 40px;
    }

    /* .el-form {
        display: inline-block;
        height: 100%;
        background-color: #fff;
        width: 100%;
        padding: 58px 0 100px 0;
    } */

    .Div4 {
        margin-top: 3.625rem;
        display: inline-block;
        width: 67%;
    }

    .fouurDiv {
        width: 31.25rem;
        /* height: 340px; */
        margin-bottom: 20px;
        font-size: 16px;
        float: left;
        text-align: left;
        margin: 50px 25px;
    }

    .fouurDiv_Tit {
        margin-bottom: 24px;
    }

    .fouurDiv_Tit>img {
        width: 20px;
        height: 20px;
        margin-bottom: -3px;
    }

    .fouurDiv_Tit>span {
        font-size: 16px;
        font-weight: bold;
        margin-left: 8px;
    }

    .fouurDiv_Tit-Bj>img {
        width: 20px;
        height: 20px;
        margin-bottom: -5px;
    }

    .fouurDiv_Tit-Bj {
        color: #999999 !important;
    }

    .fouurDiv_Cen-Li {
        margin-bottom: 20px;
        float: left;
        width: 50%;
    }

    .fouurDiv_Cen ul li {
        float: left;
    }

    .fouurDiv_Cen-Li3 {
        margin-bottom: 16px;
    }

    .fouurDiv_Cen-Li3>img {
        margin-bottom: -5px;
        width: 48px;
        height: 48px;
        margin-right: 24px;
    }

    .xuan-group img {
        margin-right: 5px;
        vertical-align: middle;
    }

    .fouurDiv_Cen-Li2 {
        width: 120px;
    }

    .fouurDiv_Cen3 {
        display: inline-block;
        width: 420px;
        background-color: #F7F6FC;
        line-height: 30px;
        padding-left: 10px;
        border-left: 2px solid #ff9933;
    }

    /* 添加房东 */

    .titleMiddle {
        margin-top: 48px;
        margin-bottom: 20px;
    }

    .titleMiddle img {
        width: 40px;
        height: 40px;
        vertical-align: text-bottom;
    }

    .titleMiddle span {
        font-size: 24px;
        font-weight: bold;
        font-style: normal;
        margin-right: 20px;
    }

    fangDong-Li {
        width: 100%;
    }

    .text {
        font-size: 16px;
        text-align: left;
    }

    .item {
        margin-bottom: 18px;
    }

    .clearfix {
        font-size: 16px;
        font-weight: bold;
    }

    .clearfix:before,
    .clearfix:after {
        display: table;
        content: "";
    }

    .clearfix:after {
        clear: both
    }

    .box-card {
        width: 100%;
        margin-bottom: 30px;
    }

    .Xq1 .Form {
        padding: 0;
    }

    .avatar-uploader .el-upload {
        border: 1px dashed #d9d9d9;
        border-radius: 6px;
        cursor: pointer;
        position: relative;
        overflow: hidden;
    }

    .avatar-uploader .el-upload:hover {
        border-color: #409EFF;
    }

    .avatar-uploader-icon {
        font-size: 28px;
        color: #8c939d;
        width: 178px;
        height: 178px;
        line-height: 178px;
        text-align: center;
    }

    .avatar {
        width: 178px;
        height: 178px;
        display: block;
    }
</style>