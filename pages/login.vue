<script lang="ts" setup>
definePageMeta({
  layout: 'empty'
});
const router = useRouter()
const { $appState } = useNuxtApp();

const email = ref('');
const password = ref('');
const checked = ref(true);


async function submit(){

    const {data} = await useAsyncGql({
    operation:"login" , 
    variables : {email: email.value , password : password.value}
});
    // const autherize = data.value.emailAndPasswordLoginBoard.data
    const token =data.value?.emailAndPasswordLoginBoard.data?.token
    if(token){
      router.push("/")
      window.localStorage.setItem("token" , token)
      window.localStorage.setItem("save" , String(checked.value))
      
    }
    
    // if(data.value)
    console.log(email.value , password.value)
    
}
// const logoColor = computed(() => ($appState.darkTheme ? 'white' : 'dark'));
</script>

<template>
  <div class="surface-0 flex align-items-center justify-content-center min-h-screen min-w-screen overflow-hidden">
    <div class="grid justify-content-center p-2 lg:p-0" style="min-width:80%">
      <div class="col-12 mt-5 xl:mt-0 text-center">
        <!-- <img :src="`/images/logo-${logoColor}.svg`" alt="Baianat logo" class="mb-5" style="width:81px; height:60px;"> -->
      </div>
      <div class="col-12 xl:col-6" style="border-radius:56px; padding:0.3rem; background: linear-gradient(180deg, grey, rgba(33, 150, 243, 0) 30%);">
        <div class="h-full w-full m-0 py-7 px-4" style="border-radius:53px; background: linear-gradient(180deg, white 38.9%, var(--surface-0));">
          <div class="text-center mb-5">
            <!-- <img src="/images/avatar/bernardodominic.png" alt="Image" height="50" class="mb-3"> -->
            <div class="text-900 text-3xl font-medium mb-3">
              Welcome, Baianater!
            </div>
            <span class="text-600 font-medium">Sign in to continue</span>
          </div>

          <div class="w-full md:w-10 mx-auto">
            
            <label for="email" class="block text-900 text-lg  mb-2 mt-2">Email</label>
            <div class="input-holder">
              <InputText type="email" id="email" placeholder="Email" v-model="email" input-class="w-full"
              style="padding:1rem 3rem ; width: 100%;" />
              <i class="pi pi-user" style="font-size: 2rem"></i> 
            </div>
   

            <label for="password" class="block text-900  text-lg mb-2 mt-2">Password</label>
            <div class="input-holder" >            
              <Password
              id="password1"
              v-model="password"
              placeholder="Password"
              :feedback="false"
              :toggle-mask="true"
              input-class="w-full"
              input-style="padding:1rem 3rem "
            
              
            />
            <i class="pi pi-lock" style="font-size: 2rem"></i> 
          </div>


            <div class="flex align-items-center justify-content-between mb-5 mt-2">
              <div class="flex align-items-center">
                <input type="checkbox" id="rememberme1" v-model="checked"  class="mr-2" />
                <label for="rememberme1">Remember me</label>
              </div>
              <a class="font-medium no-underline ml-2 text-right cursor-pointer" style="color: black">Forgot password?</a>
            </div>
            <Button label="Sign In" class="w-full p-3 text-xl p-button-raised " @click="submit()"  style="background-color:black"/>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.pi-eye {
    transform:scale(1.6);
    margin-right: 1rem;
}

.pi-eye-slash {
    transform:scale(1.6);
    margin-right: 1rem;
}
.input-holder{
position: relative;
div{
  width: 100%;
}
i{
  font-size: 1.3rem !important; 
    position: absolute;
    left: 1rem;
    top: 1rem;
}
}




</style>
