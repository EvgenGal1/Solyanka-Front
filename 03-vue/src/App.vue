<!-- описание шаблона Комп. -->
<template>
  <div>
    <!-- двухстороннее связывание (two way binding) данн. и шаблн.использ. дерективу v-model=input -->
    <input type="text" v-model="input" />
    <!-- вывод списка относительно inputa -->
    <ul>
      <!-- итерация списка filteredUsers ч/з v-for получаю u, добав.аттр.key u.id и вывод u.name -->
      <li v-for="u in filteredUsers" :key="u.id">{{ u.name }}</li>
    </ul>
  </div>
</template>

<!-- использ.современ.синаксис ч/з аттр.setup чтоб использ. композишнAPI - настр.и связь с шаблн.  -->
<script setup>
// хуки: onMounted - вызов мтд. после смонтир.Комп., ref - локал.state, computed - вычисляемое св-во
import { onMounted, ref, computed } from 'vue'

// локал.state users(масс.>с БД)/input(стр.>filter) связь с тегом input
const users = ref([])
const input = ref('')
// список зависимый от измен. users и input по filter
const filteredUsers = computed(() =>
  users.value.filter((u) =>
    u.name.toLowerCase().includes(input.value.toLowerCase())
  )
)

// после монтир.Комп.получ.данн., парсим, запись в локал.state users
onMounted(async () => {
  const res = await fetch('https://jsonplaceholder.typicode.com/users')
  users.value = await res.json()
})
</script>
