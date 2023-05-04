<template lang="pug">
.container2.p-2
  .info
    .data.flex(class=" w-1/4 justify-between")
      h3.title LOGO
      Icon(name="ci:menu-alt-04")
    .end
      button.mybutton(type='button' @click='logout()')
        i.pi.pi-sign-out
        span.ml-2.font-bold Log out
  #allTheNav(style='display: flex')
    .sidenavopen(style='display: flex')
      transition(name='side')
        .nav2(v-if='opened')
          div(style='display: flex; align-items: center; justify-content: space-between')
            i.pi.pi-times-circle(style='cursor: pointer' @click='closeside()')
          ul(style='margin-top: 20px')
            li(v-for='item in selected' :key='item.key')
              i(:class="item.icon + ' mr-2 myicon'")
              nuxt-link(:to='item.url') {{ item.label }}
  #main
    slot

</template>

<script setup>
// checking token
const route = useRouter();
const props = defineProps({
  title: String,
});
function logout() {
  window.localStorage.removeItem("token");
  route.push("login");
}
onMounted(async function check() {
  const tokenlocal = usetoken();
  console.log("local", window.localStorage.getItem("token"));
  const token = tokenlocal.value || window.localStorage.getItem("token");
  console.log("token :", token);
  useGqlToken(token);
  const { data } = await useAsyncGql({
    operation: "check",
  });
  const check = data.value?.me.message;
  console.log(check);
  if (check === "Operation done successfully") {
    if (window.localStorage.getItem("save") === "false") {
      window.localStorage.removeItem("token");
    }
    return;
  } else if (!token) {
    route.push("/login");
  }
});

const opened = ref(true);
const mykey = ref("1");
const items = reactive([
  {
    key: "0",
    label: "Dashboard",
    icon: "pi pi-home",
    url: "/",
  },

  {
    key: "1",
    label: "Users",
    icon: "pi pi-fw pi-user",
    items: [
      {
        key: "1_0",
        label: "Join requests",
        icon: "pi  pi-user-plus",
        url: "/users",
        color: "rgb(167 89 227)",
      },
      {
        key: "1_1",
        label: "Doctors",
        icon: "fa-solid fa-stethoscope",
        url: "/doctors",
        color: "#eb6f6f",
      },
    ],
  },
  {
    key: "2",
    label: "Faqs",
    icon: "pi pi-question-circle",
    items: [
      {
        key: "2_0",
        label: "Categories",
        icon: "pi pi-comments",
        url: "/faqs",
        color: "rgb(167 89 227)",
      },
    ],
  },
  {
    key: "3",
    label: "Terms and conditions",
    icon: "pi pi-question-circle",
    items: [
      {
        key: "2_0",
        label: "Terms and conditions",
        icon: "pi pi-info-circle",
        url: "/terms",
        color: "rgb(167 89 227)",
      },
    ],
  },
]);

console.log(opened.value);
const selected = ref("");
const filtered = ref("Dashboard");
const labell = ref("Dashboard");
function opendside(key) {
  // mykey.value = '0'
  filtered.value = "";

  opened.value = true;
  mykey.value = key;
  filtered.value = items.filter((e) => e.key === mykey.value);
  console.log(mykey.value);
  console.log("filtered", filtered.value);
  selected.value = filtered.value[0].items;
}
function closeside() {
  opened.value = false;
}
</script>

<style lang="scss" scoped>
.info {
  box-shadow: 0 0 6px -1px #ccc;
  margin-bottom: 10px;
  padding: 1rem;
  border-radius: 1.2rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.title {
  color: $main-color;
  font-weight: bold;
}
.subtitle {
  margin-top: 5px;
  font-size: 15px;
  color: grey;
}
.nav1 {
  height: 100vh;
  top: 0;
  position: sticky;
  background: white;
  padding: 1rem;
  box-shadow: 0 0 28px -9px #ccc;
  z-index: 1000;
  width: 80px;
  display: flex;
  justify-content: center;
  ul {
    li {
      padding: 1rem 0rem;
      cursor: pointer;
      a {
        color: grey;
        &:hover {
          color: $main-color;
        }
      }
    }
  }
}

#main {
  flex-grow: 1;
  // padding: 1rem;
  width: 70%;
  height: fit-content;
  margin: 15px 15px;
  border-radius: 1.5rem;
}

.nav2 {
  background-color: rgb(220 241 249 / 45%);
  height: 100vh;
  top: 0; /* this is required for "sticky" to work */
  width: 250px;
  position: sticky;
  padding: 1rem;
  box-shadow: 0 0 28px -9px #ccc;

  ul {
    margin-top: 20px;
    li {
      padding: 0.5rem 0rem;
      a {
        color: black;
        text-decoration: none;
        i {
          font-weight: 500;
        }
        &:hover {
          color: $main-color;
        }
      }
    }
  }
}
.myicon {
  color: white;
  background-color: $main-color;
  padding: 0.5rem;
  border-radius: 50%;
}
</style>
