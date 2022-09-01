<template>
    <el-form ref="form" :model="form" :rules="rules" label-width="120px" @validate="onValidate">
        <el-form-item label="Activity time">
            <el-col :span="11">
                <el-form-item prop="date1">
                    <el-date-picker type="date" placeholder="Pick a date" v-model="form.date1" style="width: 100%;">
                    </el-date-picker>
                </el-form-item>
            </el-col>
            <el-col class="line" :span="2">-</el-col>
            <el-col :span="11">
                <el-form-item prop="date2">
                    <el-date-picker type="date" placeholder="Pick a date" v-model="form.date2" style="width: 100%;">
                    </el-date-picker>
                </el-form-item>
            </el-col>
        </el-form-item>
    </el-form>
</template>
  <script>
  export default {
      data() {
          var validateDate1 = (rule, value, callback) => {
              console.log("date1", value)
              if (value === '' || value === null || value === undefined) {
                  callback(new Error('Please input the date1'));
              } else {
                  if (this.form.date1 !== '') {
                      this.$refs.form.validateField('date2');
                  }
                  callback();
              }
          };
  
          var validateDate2 = (rule, value, callback) => {
              console.log("date2", value)
              if (value === '' || value === null || value === undefined) {
                  callback(new Error('Please input the date2'));
              } else {
                  if (this.form.date2 < this.form.date1) {
                      callback(new Error('Date2 must later than date1'));
                  }
                  callback();
              }
          };
          return {
              form: {
                  date1: '',
                  date2: ''
              },
              rules: {
                  date1: [
                      { validator: validateDate1, trigger: 'blur' }
                  ],
                  date2: [
                      { validator: validateDate2, trigger: 'blur' }
                  ]
              }
          }
      },
      methods: {
          onValidate(a, b) {
              //   console.log("validate emit from form", a, b);
              this.$emit("validate", a, b)
          }
      }
  }
  </script>
