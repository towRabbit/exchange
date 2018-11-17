<template>
  <section class="container">
    <el-form
      ref="form"
      :model="form"
      label-width="80px">
      <el-form-item label="地址">
        <el-input v-model="form.address"/>
      </el-form-item>
      <el-form-item label="交易对id">
        <el-input v-model="form.id"/>
      </el-form-item>
      <el-form-item label="token name">
        <el-input v-model="form.ft"/>
      </el-form-item>
      <el-form-item label="token数量">
        <el-input v-model="form.fv"/>
      </el-form-item>
      <!-- <el-form-item label="s">
        <el-input v-model="form.st"/>
      </el-form-item> -->

      <el-form-item label="私钥">
        <el-input v-model="form.p"/>
      </el-form-item>
      <el-form-item>
        <el-button
          type="primary"
          @click="onSubmit">买</el-button>
      </el-form-item>
    </el-form>
  </section>
</template>

<script>
import { Client as ApiClient } from '@tronscan/client'
import Logo from '~/components/Logo.vue'

export default {
  data() {
    return {
      form: {
        address: 'TR62xys2BfUqoHGMRuTzpwG3CjAUPKCyHS',
        id: '41',
        ft: '_',
        fv: 0.03,
        st: 1,
        p: ''
      }
    }
  },
  mounted() {
    // currentWallet.address,exchangeId, tokenId, quant, expected
  },
  methods: {
    onSubmit: async function() {
      console.log(this)
      const trx = this.form.fv * Math.pow(10, 6)
      const Client = new ApiClient()
      console.log(
        this.form.address,
        this.form.id,
        this.form.ft,
        trx,
        this.form.st
      )
      const {
        success,
        code,
        transaction,
        message
      } = await Client.transactionExchange(
        this.form.address,
        this.form.id,
        this.form.ft,
        trx,
        this.form.st
      )(this.form.p)

      // 验证
      if (success) {
        this.$message.success('交易成功')
      } else {
        this.$message.error(message)
      }
    }
  }
}
</script>

<style>
.container {
}
</style>
