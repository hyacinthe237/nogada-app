<template lang="html">
    <section class="signup-page">
        <div class="g-page" :style="`background-image:url(${background})`" v-show="!isLoading">
            <div class="overlay">
                <div class="link">Already have an account? <span @click="n('signin')" class="pointer">Sign In</span></div>
            </div>
            <div class="profile-top-container">
                <div
                    class="profile-photo"
                >
                  <img :src="logo" />
                </div>
            </div>
        </div>
        <div class="d-page" v-show="!isLoading">
            <div class="profile-top" :style="`background-image:url(${background})`">
                <div class="overlay"></div>
                <div class="profile-top-container">
                    <div
                        class="profile-photo"
                    >
                      <img :src="logo" />
                    </div>
                </div>
            </div>
             <div class="dhead mt-20">
                <div class="step-title">step 1 of 4</div>
                <div class="progress"><div class="step-1"></div></div>
                <div class="message-big">Hi, We're <br> NOGODA 360</div>
            </div>

            <form class="_form s-form mt-10" @submit.prevent>
                <div class="form-group">
                    <label>What's your name?</label>
                    <input type="text" name="username" v-model="ghost.username" placeholder="e.g. Art Paul" class="form-control mt-20">
                    <div class="check-form">
                        <div class="smaller">Try everything free for 14 days. No credit card required. Basic plan is free forever.</div>
                        <input type="checkbox" name="is_free" class="checkbox" checked>
                    </div>
                </div>
                <div class="bottom-form" @click="save()">
                    <div class="notyet"></div>
                    <div class="button mt-20 pointer">
                        <div class="text">Get Started</div>
                        <div class="icon"><i class="feather icon-chevron-right"></i></div>
                    </div>
                </div>
            </form>

            <div class="link">Already have an account? <span @click="n('signin')" class="pointer">Sign In</span></div>
        </div>
        <div class="_loader" v-show="isLoading">
            <Spinners></Spinners>
        </div>
    </section>
</template>

<script>
import logo from '@/assets/img/vertical.png'
import background from '@/assets/img/background.png'
import logoUsers from '@/assets/img/users.png'
import logoApple from '@/assets/img/landing/apple.png'
import logoGoogle from '@/assets/img/landing/google-playstore.png'

export default {
    name: 'Step1',

    data: () => ({
        ghost: { username: '' },
        logo,
        showList: false,
        showForm: false,
        selected: [],
        items: [],
        background, logoUsers,logoApple,logoGoogle
    }),

    mounted () {
      this.initItems()
    },

    computed: {
    },

    methods: {
        displayList () {
            this.showList = !this.showList
        },

        displayForm () {
            this.showForm = !this.showForm
        },

        initItems () {
            this.items = [
              { 'id': 1, 'name': 'Guard scheduling' },
              { 'id': 2, 'name': 'VMS visitor management system' },
              { 'id': 3, 'name': 'Guard tracking' },
            ]
        },

        async save () {
            if (this.ghost.username == '') {
                this.$swal.error('Validation error', 'Please, fill the username.')
            }

            if (this.ghost.username !== '') {
                this.startLoading()
                const response = await this.$api.post('user-api/step-1/manager', this.ghost)
                .catch(error => {
                    console.log('Error ==> ', error.response.data)
                    this.stopLoading()
                    this.$swal.error('Step 1 Error', error.response.data.error_message)
                })
            
                this.stopLoading()
                if(response) {
                    localStorage.setItem('business', JSON.stringify(response.data))
                    // this.$swal.success('Confirmation', 'Projet ajouté')
                    this.n('signup-step-two')
                }
            }
        }
    }
}
</script>
