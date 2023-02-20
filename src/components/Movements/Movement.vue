<template>
    <div class="movement">
        <div class="content">
            <h4>{{ title }}</h4>
            <h4>{{ description }}</h4>
        </div>
        <div class="action">
            <img src="@/assets/trash-icon.svg" alt="borrar" @click="remove"/>
            <p :class="{red: isNegative, 'green': !isNegative}">{{ amountCurrency }}</p>
            <!-- basicamente hace esto: :class="{'red': true, 'green': false} -->
        </div>
    </div>
</template>

<script setup>
import {defineProps, toRefs, computed, defineEmits} from 'vue';

const currencyFormater = new Intl.NumberFormat(("es-CO"), {
  style: "currency",
   currency: "COP"
})

const props = defineProps(["title", "description","amount","id"])

const {id, title, description, amount} = toRefs(props)

const amountCurrency = computed(() => {
    return currencyFormater.format(amount.value)
})

const emit = defineEmits(["remove"])

const remove = () => {
    console.log("IDD",id.value)
    emit("remove",id.value)
}

const isNegative = computed(() => {
    return amount.value < 0
})

</script>

<style scoped>
.movement {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  padding: 16px;
  background-color: #e6f9ff;
  border-radius: 8px;
  box-sizing: border-box;
}
.movement .content {
  width: 100%;
}
.movement .action {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  flex-direction: column;
}
h4,
p {
  margin: 0;
  padding: 0;
}
h4 {
  margin-bottom: 8px;
}
.movement .action img {
  margin-bottom: 16px;
}
.red {
    color: red;
}
.green {
    color: green;
}
</style>