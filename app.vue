<script setup>
import 'tdesign-vue-next/es/style/index.css'
import './styles/theme.css'

onBeforeMount(async () => {
  // this is essential to make request sent at client side
  // should be a bug of nuxt

  await nextTick()

  const { host, protocol } = location
  if (host === 'nuistshare.cn')
    location.href = 'https://www.nuistshare.cn'

  if (protocol === 'http:' && !host.includes('localhost'))
    location.href = `https://${host}`

  const login = useLogin()
  const user = useUser()
  const token = localStorage.getItem('token')
  if (token) {
    const id = getUserId(token)
    login.value.userId = id
    login.value.lsLoggedIn = true
    login.value.token = token
    const res = await useApi('/api/users/me')
    user.value = res.data.value
  }
})
</script>

<template>
  <NsHeader />
  <main>
    <NuxtPage />
  </main>
  <NsFooter />
</template>

<style>
html,
body {
  margin: 0 !important;
  padding: 0 !important;
}

html {
  font-family: "PingFang SC";
  background-color: #f5f5f5;
  scroll-behavior: smooth;
}

:target {
  scroll-margin-top: 4rem;
}

a {
  color: unset;
}

.title-font {
  font-family: "YouSheBiaoTiHei";
  font-weight: 500 !important;
  text-wrap: balance;
}

::selection{
  background-color: rgb(75, 75, 75);
  color: #f5f5f5;
  border-radius: 5px;
}
</style>
