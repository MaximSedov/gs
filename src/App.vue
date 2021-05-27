<template>
  <div class="trainer">
    <el-row type="flex" justify="center" align="middle">
      <el-col :xs="22" :sm="22" :md="24" :lg="10" :xl="10">
        <h1>Тренажер "Запомни число"</h1>
        <span class="num">{{ num }}</span>
        <el-form ref="startForm" :model="startForm" label-width="120px" :rules="rules" class="demo-ruleForm" label-position="top">
          <el-row>
            <el-col :xs="24" :sm="24" :md="24" :lg="12" :xl="12">
              <el-form-item label="Разрядность" prop="digit">
                <el-select v-model="startForm.digit" placeholder="Выбери разрядность числа">
                  <el-option label="1 разряд" value="3"></el-option>
                  <el-option label="2 разряд" value="4"></el-option>
                  <el-option label="3 разряда" value="5"></el-option>
                  <el-option label="4 разряда" value="6"></el-option>
                  <el-option label="5 разрядов" value="7"></el-option>
                </el-select>
              </el-form-item>
            </el-col>
            <el-col :xs="0" :sm="0" :md="0" :lg="1" :xl="1">
            </el-col>
            <el-col :xs="24" :sm="24" :md="24" :lg="11" :xl="11">
              <el-form-item label="Время" prop="time">
                <el-input-number v-model="startForm.time" :step="0.1" :min="0.2" :max="2.0"></el-input-number>
              </el-form-item>
            </el-col>
          </el-row>

          <el-form-item>
            <el-button type="primary" @click="submitStartForm('startForm')">Старт</el-button>
          </el-form-item>
        </el-form>

        <el-form v-show="visible" ref="checkForm" :model="checkForm" label-width="120px" :rules="rules" class="demo-ruleForm" label-position="top">
          <el-row>
            <el-col :xs="24" :sm="24" :md="24" :lg="12" :xl="12">
              <el-form-item label="Введи число" prop="num">
                <el-input type="number" placeholder="Постарайся вспомнить!" v-model="checkForm.num"></el-input>
              </el-form-item>
            </el-col>
          </el-row>

          <el-form-item>
            <el-button type="primary" @click="submitCheckForm('checkForm')">Проверить</el-button>
          </el-form-item>
        </el-form>
      </el-col>
    </el-row>
  </div>
</template>
<script>
export default {
  data() {
    return {
      num: '',
      numCheck: '',
      visible: false,
      startForm: {
        name: '',
        digit: '',
        time: 1
      },
      checkForm: {
        num: ''
      },
      rules: {
        num: [
          {
            required: true,
            message: 'Не-а! Сначала введи число!',
            trigger: 'blur'
          },
          {
            min: 1,
            max: 5,
            message: 'Длина от 1 до 5! XD',
            trigger: 'blur'
          }
        ],
        digit: [
          {
            required: true,
            message: 'Не-а! Сначала выбери разрядность!',
            trigger: 'blur'
          }
        ],
        time: [
          {
            required: true,
            message: 'Не-а! Сначала выбери время показа!',
            trigger: 'blur'
          }
        ]
      }
    }
  },
  methods: {
    submitStartForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          window.clearTimeout
          let digit = this.startForm.digit
          let time = this.startForm.time * 1000
          let randomNum = Math.random()
            .toString()
            .slice(2, digit)
          this.num = randomNum
          this.numCheck = randomNum
          this.visible = true
          setTimeout(() => (this.num = ''), time)
        } else {
          return false
        }
      })
    },
    submitCheckForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          if (this.checkForm.num == this.numCheck) {
            this.$message({
              message: 'Правильно!',
              showClose: true,
              type: 'success'
            })
            this.checkForm.num = ''
            this.visible = false
          } else {
            this.$message({
              message: 'Неправильно!',
              showClose: true,
              type: 'warning'
            })
          }
        } else {
          return false
        }
      })
    }
  }
}
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@400;700&display=swap");
* {
  font-family: "Montserrat", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  padding: 0;
  margin: 0;
}
.trainer {
  h1 {
    text-align: center;
    text-transform: uppercase;
    font-size: 20px;
    color: #409eff;
    margin-top: 2rem;
  }
  .num {
    font-size: 60px;
    text-align: center;
    display: block;
    margin-bottom: 1rem;
    margin-top: 1rem;
    font-weight: bold;
    color: #409eff;
    height: 60px;
  }
}

.el-input {
  width: 100%;
}
.el-select {
  width: 100%;
}
.el-input-number {
  width: 100% !important;
}
input[type="number"]::-webkit-outer-spin-button,
input[type="number"]::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
input[type="number"] {
  -moz-appearance: textfield;
}
input[type="number"]:hover,
input[type="number"]:focus {
  -moz-appearance: number-input;
}
input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
</style>
