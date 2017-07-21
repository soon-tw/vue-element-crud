<template>
    <el-card class="box-card">
        <el-tabs v-model="activeName2" type="card" @tab-click="handleClick">
            <el-tab-pane label="查詢員工" name="first">
                <p>
                    <el-button @click="getEmp" type="primary">查 詢</el-button>
                    <el-button @click="dialogFormVisible = true" type="primary">新 增</el-button>
                </p>
                <el-table :data="Datas" height="600" border style="width: 100%" :default-sort="{prop: 'id', order: 'descending'}">
                    <el-table-column prop="id" label="序號" sortable width="120"> </el-table-column>
                    <el-table-column prop="name" label="姓名" sortable width="200"> </el-table-column>
                    <el-table-column prop="email" label="電子郵件" sortable width="200"> </el-table-column>
                    <el-table-column prop="contact_number" label="電話" width="200"> </el-table-column>
                    <el-table-column prop="position" label="地址" sortable> </el-table-column>
                    <el-table-column prop="created_at" label="建檔時間" width="220"> </el-table-column>
    
                    <el-table-column fixed="right" label="操作" width="100">
                        <template scope="scope">
                                        <el-button @click="editEmp(scope.row)" type="text" size="small">編輯</el-button>
</template>
                    </el-table-column>

                </el-table>
                
            </el-tab-pane>
            <el-tab-pane label="新增員工" name="second">
                <el-form ref="form" :model="form" label-width="80px">
                    <el-form-item label="姓名">
                        <el-input v-model="form.name"></el-input>
                    </el-form-item>
                    <el-form-item label="電子郵件">
                        <el-input v-model="form.email"></el-input>
                    </el-form-item>
                    <el-form-item label="電話">
                        <el-input v-model="form.contact_number"></el-input>
                    </el-form-item>
                    <el-form-item label="地址">
                        <el-input v-model="form.position"></el-input>
                    </el-form-item>
    
                    <el-form-item>
                        <el-button type="primary" plain @click="onSubmit">新增資料</el-button>
                        <el-button>取消</el-button>
                        <el-button plain @click="success">
                            成功
                        </el-button>
                    </el-form-item>
                </el-form>
            </el-tab-pane>
            <el-tab-pane label="修改員工" name="third">
                <el-form ref="form" :model="editForm" label-width="80px">
                    <el-form-item label="姓名">
                        <el-input v-model="editForm.name"></el-input>
                    </el-form-item>
                    <el-form-item label="電子郵件">
                        <el-input v-model="editForm.email"></el-input>
                    </el-form-item>
                    <el-form-item label="電話">
                        <el-input v-model="editForm.contact_number"></el-input>
                    </el-form-item>
                    <el-form-item label="地址">
                        <el-input v-model="editForm.position"></el-input>
                    </el-form-item>
    
                    <el-form-item>
                        <el-button type="primary" plain @click="editSave">修改</el-button>
                        </el-button>
                    </el-form-item>
                </el-form>
            </el-tab-pane>
            <el-tab-pane label="定时任务补偿" name="fourth">定时任务补偿</el-tab-pane>
        </el-tabs>


        <el-dialog title="員工資料" :visible.sync="dialogFormVisible">
            <el-form :model="form">
                <el-form-item label="姓名" :label-width="formLabelWidth">
                    <el-input v-model="form.name" auto-complete="off"></el-input>
                </el-form-item>
                <el-form-item label="電子郵件" :label-width="formLabelWidth">
                    <el-input v-model="form.email" auto-complete="off"></el-input>
                </el-form-item>
                <el-form-item label="電話" :label-width="formLabelWidth">
                    <el-input v-model="form.contact_number" auto-complete="off"></el-input>
                </el-form-item>
                <el-form-item label="地址" :label-width="formLabelWidth">
                    <el-input v-model="form.position" auto-complete="off"></el-input>
                </el-form-item>
              
                </el-form-item>
            </el-form>
            <div slot="footer" class="dialog-footer">
                <el-button type="primary" @click="setEmpInfo">存 檔</el-button>
                <el-button @click="dialogCancel">取 消</el-button>
            </div>
        </el-dialog>


    </el-card>
    
    <!--<el-form ref="form" :model="form" label-width="80px">
                                        <el-form-item label="姓名">
                                            <el-input v-model="form.name"></el-input>
                                        </el-form-item>
                                        <el-form-item label="電子郵件">
                                            <el-input v-model="form.email"></el-input>
                                        </el-form-item>
                                        <el-form-item label="電話">
                                            <el-input v-model="form.contact_number"></el-input>
                                        </el-form-item>
                                        <el-form-item label="地址">
                                            <el-input v-model="form.position"></el-input>
                                        </el-form-item>
                                    
                                        <el-form-item>
                                            <el-button type="primary" plain @click="onSubmit">新增資料</el-button>
                                            <el-button>取消</el-button>
                                            <el-button plain @click="open3">
                                                成功
                                            </el-button>
                                        </el-form-item>
                                    </el-form>-->
</template>

<script>
    import axios from 'axios'
    
    export default {
        name: 'apiAdd',
        data() {
            return {
                title: 'Laravel Api 後端呼叫練習/ 表單新增（專案：ApiAdd.vue）',
                apiUrl: 'http://localhost:8888/api/v1/employees',
                Datas: [],
                dialogFormVisible: false,
                formLabelWidth: '120px',
                form: {
                    id: '',
                    name: '',
                    email: '',
                    contact_number: '',
                    position: ''
                },
                editForm: {
                    id: '',
                    name: '',
                    email: '',
                    contact_number: '',
                    position: ''
                },
                activeName2: 'first'
            }
        },
        methods: {
            onSubmit() {
    
                console.log(this.form)
    
                axios.post(this.apiUrl, {
                        name: this.form.name,
                        email: this.form.email,
                        contact_number: this.form.contact_number,
                        position: this.form.position
                    })
                    .then(response => {
                        this.success()
                        this.activeName2 = 'first'
                        this.getEmp()
                        this.initialization()
                        console.log(response)
    
                    })
                    .catch(error => {
                        this.errMsg()
                        console.log(error);
                    })
    
            },
            success() {
                this.$notify({
                    title: '成功',
                    message: '恭喜您，資料存檔成功',
                    type: 'success'
                });
            },
            errMsg() {
                this.$notify.error({
                    title: '错误',
                    message: '存檔失敗'
                });
            },
            handleClick(tab, event) {
                console.log(tab, event);
            },
            getEmp() {
                this.loading = true
                axios.get(this.apiUrl)
                    .then(response => {
                        this.Datas = response.data
                    })
                    .catch(e => {
                        this.errors.push(e)
                    })
            },
            initialization() {
                this.form.id = ''
                this.form.name = ''
                this.form.email = ''
                this.form.contact_number = ''
                this.form.position = ''
            },
            initEditForm() {
                this.editForm.id = ''
                this.editForm.name = ''
                this.editForm.email = ''
                this.editForm.contact_number = ''
                this.editForm.position = ''
            },
            editEmp(row) {
                this.form.id = row.id
                this.form.name = row.name
                this.form.email = row.email
                this.form.contact_number = row.contact_number
                this.form.position = row.position
                this.dialogFormVisible = true;
                console.log(row);
            },
            editSave() {
                axios.post(this.apiUrl + '/' + this.editForm.id, {
                        name: this.editForm.name,
                        email: this.editForm.email,
                        contact_number: this.editForm.contact_number,
                        position: this.editForm.position
                    })
                    .then(response => {
                        this.success()
                        this.getEmp()
                        console.log(response)
    
                    })
                    .catch(error => {
                        this.errMsg()
                        console.log(error);
                    })
    
                this.activeName2 = 'first'
                this.initEditForm()
    
            },
            print(h, w) {
    
                console.log("ＢＭＩ：" + w / ((h / 100) ^ 2))
            },
            /**
             * 新增修改員工資訊
             */
            setEmpInfo() {
                this.apiUrl = this.form.id == '' ? this.apiUrl : (this.apiUrl + '/' + this.form.id)
                axios.post(this.apiUrl, {
                        name: this.form.name,
                        email: this.form.email,
                        contact_number: this.form.contact_number,
                        position: this.form.position
                    })
                    .then(response => {
                        this.success() // 秀成功訊息
                        this.dialogCancel()
                        this.getEmp()
                        console.log(response)
    
                    })
                    .catch(error => {
                        this.errMsg()
                        console.log(error);
                    })
            },
            /**
             * 關掉彈跳視窗
             */
            dialogCancel(){
                this.dialogFormVisible = false
                this.initialization() // 初始化
            }
    
    
        }
    }
</script>