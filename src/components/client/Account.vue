<script setup>

import Transaction from "@/components/Transaction.vue";
import Deposit from "@/components/client/Deposit.vue";
import Withdraw from "@/components/client/Withdraw.vue";
import Transfer from "@/components/client/Transfer.vue";
import Pay from "@/components/client/Pay.vue";
</script>
<script>
import axios from "axios";
import ClientSearch from "@/components/admin/ClientSearch.vue";
import Transaction from "@/components/Transaction.vue";
import ManagerMenu from "@/components/admin/ManagerMenu.vue";
import CreateAccount from "@/components/admin/CreateAccount.vue";
import Interest from "@/components/admin/Interest.vue";

export default {
  props: ["accountId", "admin", "clientId"],
  data() {
    return {
      message: {},
      account: null,
      accountView: Transaction,
      balance: "",

    };
  },
  methods: {

    accountInfo() {
      fetch("http://localhost:8080/accountinfo", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: this.accountId.toString(),
      })
          .then(response => response.json())
          .then(data => {
            this.account = data;
            this.balance = data.balance;
            if ('id' in data) {
              console.log("ID found in response:", data.id);
            } else {
              console.log("ID not found in the response.");
            }
          })
          .catch(error => {
            console.error("Error:", error);
          });
    },
    accountHeaderType() {
      let response
      switch (this.account.type) {
        case "1": response = "checking";break;
        case "2": response = "saving"; break;
        case "3": response = "mortgage"; break;
        case "4": response = "line of credits"; break;
      }
      return response
    },
    changeAccountView(view){
      this.accountView = view;
    },
    updateBalance(balance) {
      console.log(balance);
      this.balance = balance;

    }
  },
  watch: {
    accountId: {
      immediate: true,
      handler(newAccountId) {
        this.accountInfo(newAccountId);
        this.accountView = Transaction;
      }
    }
  },
  mounted() {
    this.accountInfo();
  },
};
</script>
<template>
  <div class="accountContainer">
    <div class="accountHeader">
      <p>type</p>
      <p>{{ accountHeaderType() }}</p>
    </div>
    <div class="accountHeader">
    <p>name</p>
  <p>{{this.account.name}}</p>
  </div>
  <div class="accountHeader">
    <p>balance</p>
    <p>{{ balance }}</p>
  </div>
  </div>

  <div v-if="!this.admin">
    <button
        @click="changeAccountView(Deposit)"
        v-if="['1', '2', '3'].includes(account.type)"
        class="accountButton">
      deposit
    </button>
    <button
        @click="changeAccountView(Withdraw)"
        v-if="['1', '2'].includes(account.type)"
        class="accountButton">
      withdraw
    </button>
    <button
        @click="changeAccountView(Transfer)"
        v-if="['1'].includes(account.type)"
        class="accountButton">
      transfer
    </button>
    <button
        @click="changeAccountView(Pay)"
        v-if="['1'].includes(account.type)"
        class="accountButton">
      Pay
    </button>
    <button
        @click="changeAccountView(Transaction)"
        class="accountButton">
      transaction
    </button>
  </div>
  <div v-if="this.admin">
    <button
        @click="changeAccountView(Withdraw)"
        v-if="['3'].includes(account.type)"
        class="accountButton">
      withdraw
    </button>
    <button
        @click="changeAccountView(Transaction)"
        class="accountButton">
      transaction
    </button>
  </div>
  <div>
    <component :is="accountView" :accountId="accountId" :clientId="clientId" @updateBalance="updateBalance"/>
  </div>

</template>

<style scoped>
.accountContainer {
  display: flex;
  justify-content: space-between;
  gap: 20px;
}
.accountHeader {
  flex: 1;
  padding: 10px;
  box-sizing: border-box;
}
.accountButton {
  width: 120px;
}
</style>