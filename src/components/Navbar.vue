<template lang="">
    <div>
        <nav
            class="navbar has-shadow is-light"
            role="navigation"
            aria-label="main navigation"
        >
            <div class="navbar-brand">
                <a class="navbar-item" href="/">
                    <img
                        src="../assets/logo.png"
                        width="28"
                        height="28"
                        alt="logo"
                    />
                </a>
                <a class="navbar-item is-spaced">
                    Chess
                </a>
            </div>

            <div class="navbar-menu">
                <div class="navbar-end">
                    <div class="navbar-item" v-show="showGameEntry">
                        <button class="button is-light" @click="gotoPlay">
                            进入游戏
                        </button>
                    </div>
                    <div class="navbar-item has-dropdown is-hoverable">
                        <a class="navbar-link"> Welcome! {{ username }} </a>

                        <div class="navbar-dropdown is-right">
                            <a class="navbar-item">
                                用户信息
                            </a>
                            <router-link to="/history" class="navbar-item">
                                历史对局
                            </router-link>
                            <!-- </a> -->
                            <hr class="navbar-divider" />
                            <a class="navbar-item" @click="quit">
                                退出登录
                            </a>
                        </div>
                    </div>
                </div>
            </div>
        </nav>
    </div>
</template>

<script lang="ts">
    import Vue from "vue";
    import store from "@/store/index";
    export default Vue.extend({
        name: "Navbar",
        data() {
            return {
                username: "",
            };
        },
        props: {
            showGameEntry: {
                type: Boolean,
                default: false,
            },
        },
        mounted() {
            let data = store.getters.getUser;
            this.username = data.username || "";
        },
        methods: {
            quit() {
                this.$emit("quit");
            },
            gotoPlay() {
                this.$router.push("game");
            },
        },
    });
</script>
