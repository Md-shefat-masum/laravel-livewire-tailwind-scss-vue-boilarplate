<template>
    <div v-if="get_check_auth">
        <top-nav></top-nav>
        <main-menu></main-menu>
        <!-- BEGIN: Content-->
        <div class="app-content content">
            <div class="content-wrapper container-xxl p-0">
                <!-- <bread-cumb></bread-cumb> -->
                <div class="content-body">
                    <router-view></router-view>
                </div>
            </div>
        </div>

        <!-- END: Content-->
        <div class="loader export_loader">
            <div class="loader_body">
                <div class="progress"></div>
                <div class="load_amount">
                    <h4>0</h4>
                    <h5>%</h5>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import BreadCumb from "./layouts/main_body/BreadCumb.vue";
import MainMenu from "./layouts/main_menu/MainMenu.vue";
import TopNav from "./layouts/TopNav.vue";
import { mapActions, mapGetters } from "vuex";
export default {
    components: { TopNav, MainMenu, BreadCumb },
    created: function () {
        this.fetch_check_auth();
    },
    watch: {
        get_check_auth: {
            handler: function (newv, oldv) {
                // console.log(newv);
                // setTimeout(() => {
                // $('.navigation li a.active').parents('li.has-sub').addClass('open');
                // }, 500);

                if (!newv) {
                    localStorage.removeItem("token");
                    location.href = "/login";
                }

                if(this.role_names.includes('admin') && this.$route.name === "Dashboard"){
                    this.$router.replace('/admin')
                }
            },
            deep: true,
        },
    },
    methods: {
        ...mapActions(["fetch_check_auth"]),
    },
    computed: {
        ...mapGetters({
            get_check_auth : "get_check_auth",
            auth_info: "get_auth_information",
        }),
        role_names: function(){
            return this.auth_info.roles.map((i)=>i.name);
        },
        role_serials: function(){
            return this.auth_info.roles.map((i)=>i.role_serial);
        },
    },
};
</script>

<style>
</style>
