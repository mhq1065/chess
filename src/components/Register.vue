<script lang="ts">
    import Vue from "vue";
    import { register } from "@/utils/user";
    import modal from "./Modal.vue";
    import popup from "@/components/popup.vue"; // @ is an alias to /src

    export default Vue.extend({
        name: "Register",
        data: function() {
            return {
                username: "",
                pwd: "",
                confirmPwd: "",
                isShowPopup: false,
                isShowModal: false,
                modalMsg: "",
            };
        },
        computed: {
            isValid: function(): boolean {
                return this.pwd === this.confirmPwd;
            },
        },
        components: {
            popup,
            modal,
        },
        methods: {
            showPopup: function(msg: string) {
                this.modalMsg = msg;
                this.isShowPopup = true;
                setTimeout(() => {
                    this.isShowPopup = false;
                }, 1000);
            },
            showModal: function() {
                this.isShowModal = true;
            },
            reg: async function() {
                if (this.username == "") {
                    this.showPopup("用户名不能为空");
                    return;
                } else if (this.pwd == "") {
                    this.showPopup("密码不能为空");
                    return;
                } else if (this.pwd !== this.confirmPwd) {
                    this.showPopup("密码不一致");
                    return;
                }

                try {
                    let data = await register(this.username, this.pwd);
                    this.showModal();
                    console.log(data);
                } catch (e) {
                    this.showPopup(e);
                    console.log("注册失败", e);
                }
            },
        },
    });
</script>

<template>
    <div class="container ">
        <!-- 用户名 -->
        <div class="field">
            <div class="control has-icons-left has-icons-right">
                <input
                    class="input is-success"
                    type="text"
                    placeholder="用户名"
                    v-model="username"
                />
                <span class="icon is-small is-left">
                    <i class="fas fa-user"></i>
                </span>
            </div>
        </div>
        <!-- 密码 -->
        <div class="field">
            <p class="control has-icons-left">
                <input
                    class="input"
                    type="password"
                    placeholder="密码"
                    v-model="pwd"
                />
                <span class="icon is-small is-left">
                    <i class="fas fa-lock"></i>
                </span>
            </p>
        </div>
        <!-- 确认密码 -->
        <div class="field">
            <div class="control has-icons-left has-icons-right">
                <input
                    class="input"
                    :class="{ 'is-danger': !isValid }"
                    type="password"
                    placeholder="确认密码"
                    v-model="confirmPwd"
                />
                <span class="icon is-small is-left">
                    <i class="fas fa-lock"></i>
                </span>
                <!-- 警告 -->
                <span v-show="!isValid" class="icon is-small is-right">
                    <i class="fas fa-exclamation-triangle"></i>
                </span>
                <!-- check -->
                <span v-show="isValid" class="icon is-small is-right">
                    <i class="fas fa-check"></i>
                </span>
            </div>
            <p class="help is-danger" v-show="!isValid">确认密码不一致</p>
        </div>

        <!-- 登录 -->
        <div class="field is-grouped">
            <div class="control">
                <button class="button is-link" @click="reg">注册</button>
            </div>
        </div>
        <modal :isShow="isShowModal" @closeModal="isShowModal = false">
            注册成功，请进行登录操作
        </modal>
        <popup :isShow="isShowPopup">
            {{ modalMsg }}
        </popup>
    </div>
</template>
