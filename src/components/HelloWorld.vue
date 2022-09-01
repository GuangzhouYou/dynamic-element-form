<template>
  <el-form :model="dynamicValidateForm" ref="dynamicValidateForm" label-width="120px" class="demo-dynamic">
    <el-form-item prop="email" label="Email" :rules="[
      { required: true, message: 'Please input email address', trigger: 'blur' },
      { type: 'email', message: 'Please input correct email address', trigger: ['blur', 'change'] }
    ]">
      <el-input v-model="dynamicValidateForm.email"></el-input>
    </el-form-item>
    <!-- <el-form-item v-for="(domain, index) in dynamicValidateForm.domains" :label="'Domain' + index" :key="domain.key"
      :prop="'domains.' + index + '.value'" :rules="{
        required: true, message: 'domain can not be null', trigger: 'blur'
      }"> -->
    <el-form-item v-for="(domain, index) in dynamicValidateForm.domains" :label="'Domain' + index" :key="domain.key"
      :prop="'domains.' + index + '.value'">
      <!-- <el-input v-model="domain.value"></el-input> -->
      <DateRangeForm :ref="'date' + index" @validate="onValidate" />
      <el-button @click.prevent="removeDomain(domain)">Delete</el-button>
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="submitForm('dynamicValidateForm')">Submit</el-button>
      <el-button @click="addDomain">New domain</el-button>
      <el-button @click="resetForm('dynamicValidateForm')">Reset</el-button>
    </el-form-item>
  </el-form>
</template>
<script>
import DateRangeForm from './DateRangeForm.vue';

export default {
  components: {
    DateRangeForm
  },
  data() {
    return {
      dynamicValidateForm: {
        domains: [{
          key: 1,
          value: ''
        }],
        email: ''
      }
    };
  },
  methods: {
    onValidate(prop, result) {
      console.log("on validate: ", prop, result);
    },
    submitForm(formName) {
      const len = this.dynamicValidateForm.domains.length;
      let pass = true;
      for (let i = 0; i < len; i++) {
        let name = 'date' + i;
        this.$refs[name][0].$refs.form.validate((valid) => {
          if (!valid) {
            console.log('error date ' + i);
            pass = false;
            // this.dynamicValidateForm.domains[i].value = null;
            return false;
          } else {
            // this.dynamicValidateForm.domains[i].value = "1";
          }
        });
      }

      this.$refs[formName].validate((valid) => {
        if (pass && valid) {

          alert('submit!');
        } else {
          console.log('error submit!!');
          return false;
        }
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
      const len = this.dynamicValidateForm.domains.length;
      for (let i = 0; i < len; i++) {
        let name = 'date' + i;
        this.$refs[name][0].$refs.form.resetFields();
      }
    },
    removeDomain(item) {
      var index = this.dynamicValidateForm.domains.indexOf(item);
      if (index !== -1) {
        this.dynamicValidateForm.domains.splice(index, 1);
      }
    },
    addDomain() {
      this.dynamicValidateForm.domains.push({
        key: Date.now(),
        value: ''
      });
    }
  }
}
</script>
