<template>
    <div>
        <!-- <h3 class="elementIdentifier">component navbar</h3> -->

        <nav class="navbar navbar-expand-lg navbar-light bg-light p-2 javNavbar1">
            <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarTogglerDemo01" aria-controls="navbarTogglerDemo01" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse japNavBar2" id="navbarTogglerDemo01" >
                <a class="navbar-brand" href="#" 
                    @click.prevent="goToHomePage"
                    style="font-family: Varela Round', sans-serif;">HACKCOMMERCE-8</a>
                <ul class="navbar-nav mr-auto mt-2 mt-lg-0">
                    <li class="nav-item active" v-if="isLogin">
                        <button 
                            class="btn btn-light" 
                            @click.prevent="goToUserShopPage">
                            <font-awesome-icon icon="store" style="color:grey"/>
                            <span style="color:grey">my Shop</span>
                        </button>
                    </li>
                    <li class="nav-item" v-if="isLogin">
                        <b-button 
                            class="btn btn-light"
                            v-b-tooltip.hover title="view my carts"
                            @click.prevent="goToMyCartsPage">
                            <font-awesome-icon icon="shopping-cart" style="color:#00b7ba"/>
                        </b-button>
                        
                        
                    </li>
                </ul>
                <form class="form-inline my-2 my-lg-0 japNavBar3" @submit.prevent="goToSearchResultPage" >
                    <input class="form-control " 
                        type="search" placeholder="Search Item" 
                        aria-label="Search" style="width:85%" 
                        v-model="searchByNameString" required>
                    <button
                        class="btn btn-outline-success my-2 my-sm-0" 
                        type="submit"
                        >Search
                    </button>
                </form>
                <!-- <div style="margin-left:8px">
                    <button class="btn btn-secondary" >Log Out</button>
                </div> -->
                <!-- start of 'user dropdown' -->
                <div class="japNavBar4">
                    <ul class="navbar-nav mr-auto" style="margin-right:10%">
                    <li v-if="!isLogin" class="japNavBar5">
                        <button 
                            type="button" 
                            class="btn btn-outline-success"
                            @click.prevent='goToLoginPage'>Login
                        </button>
                    </li>
                    <li v-if="!isLogin" class="japNavBar5">
                        <button 
                            type="button" 
                            class="btn btn-success"
                            @click.prevent="goToRegistrationPage">Register
                        </button>
                    </li>
                    <li v-if="isLogin" class="nav-item dropdown japNavBar5">
                        <div>
                            <b-dropdown id="dropdown-right" right :text="loggedInUserDetail.username" variant="secondary" >
                                <div style="text-align:right">
                                    <b-dropdown-item-button @click.prevent="goToUserProfilePage">Edit Profile</b-dropdown-item-button>
                                    <b-dropdown-item-button @click.prevent="goToProductPage">Add New Product</b-dropdown-item-button>
                                    <b-dropdown-item-button @click.prevent="goToDashboardPage">Dashboard</b-dropdown-item-button>
                                    <b-dropdown-divider></b-dropdown-divider>
                                    <b-dropdown-item-button @click.prevent="logOut">Log Out</b-dropdown-item-button>
                                </div>
                            </b-dropdown>
                        </div>
                    </li>
                    </ul>
                </div>
                <!-- end of 'user dropdown' -->
            </div>
        </nav>
    </div>
    
</template>

<script>
import axios from '../../config/axios'
import swal from 'sweetalert2'
import { mapGetters } from 'vuex'

export default {
    name: 'comp-NavBar',
    data(){
        return {
            searchByNameString:''
        }
    },
    methods:{
        goToHomePage(){
            this.$router.push('/')  
        },
        goToUserShopPage(){
            this.$router.push(`/userShop/${this.loggedInUserDetail._id}`)  
        },
        goToMyCartsPage(){
            this.$router.push('/myCarts')  
        },
        goToSearchResultPage(){
            this.searchByName()
            this.$router.push('/searchResult')  
        },
        goToRegistrationPage(){
            this.$router.push('/user/registration')  
        },
        goToLoginPage(){
            this.$router.push('/user/login')  
        },
        goToUserProfilePage(){
            this.$router.push('/userprofile')
        },
        goToDashboardPage(){
            this.$router.push('/dashboard/invoiceIncomplete')
        },
        goToProductPage(){
            this.$router.push('/product/create')
        },
        logOut(){
            this.$store.dispatch('logOut')
        },
        searchByName(){
            if(this.searchByNameString){
                axios({
                    method: 'post',
                    url: `/items/filter`,
                    data:{
                        name : this.searchByNameString
                    }
                })
                .then( ({data}) =>{
                    console.log(`TCL: searchByName -> data`, data)
                    this.searchByNameString = ''
                    this.$store.commit('SET_SEARCH_ITEM_ARRAY', data)
                })
                .catch( ({response}) =>{
                console.log(' error @registration-RegistrationLoginPage \n======================\n', response.data)
                swal.fire(
                    'Error With Searching Item',
                    response.data.message
                )
            })
            }

        }
    },
    computed:{
        ...mapGetters([
            'isLogin',
            'loggedInUserDetail',
        ])
    }




}
</script>

<style scoped>

.javNavbar1{
    position: static;
    /* border:solid 2px red; */
    /* position: sticky; */
    /* position: absolute; */
    /* position: fixed; */
    /* border: solid 2px red; */
    /* padding-top:1%;
    padding-bottom:1%; */
    height: 20%;
    top: 0;

}

.japNavBar2{
    /* border: solid 2px blue; */
    width: 100%
}

.japNavBar3{
    /* border: solid 2px blue; */
    width: 50%;
    margin:auto
}

.japNavBar4{
    margin-right:2%
}

.japNavBar5{
    /* border:solid 2px red; */
    display: flex;
    margin-right: 3%;
}
</style>