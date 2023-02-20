<template>
  <Layout>
    <template #header>
      <Header></Header>
    </template>
    <template #resume>
      <Resume
        :total-label="'Ahorro total'"
        :label="label"
        :amount="amount"
        :totalAmount="totalAmount"
      >
        <template #graphic> <Graphic :amounts="amounts" @select="select" /> </template>
        <template #action> <Action @create="create" /> </template>
      </Resume>
    </template>
    <template #movements>
      <Movements 
        :movements="movements" 
        @remove="remove"
      />
    </template>
  </Layout>
</template>

<script>
import Layout from "./Layout.vue";
import Header from "./Header.vue";
import Resume from "./Resume/Index.vue";
import Movements from "./Movements/Index.vue";
import Action from "./Action.vue";
import Graphic from "./Resume/Graphic.vue";

export default {
  components: {
    Action,
    Layout,
    Graphic,
    Header,
    Resume,
    Movements,
  },
  data(){
    return {
      label: null,  
      amount: null,
      amounts: [],
      movements: [],
      // amounts: [100, 200, 500, 200, -400, -600, -300, 0, 300, 500],
      // movements: [
      //   {
      //     id: 0,
      //     title: "Movimiento 1",
      //     description: "Lorem ipsum dolor sit amet",
      //     amount: 100,
      //     time: new Date("02-01-2023"),
      //   },
      //   {
      //     id: 1,
      //     title: "Movimiento 2",
      //     description: "Lorem ipsum dolor sit amet",
      //     amount: 200,
      //     time: new Date("02-01-2023"),
      //   },
      //   {
      //     id: 2,
      //     title: "Movimiento 3",
      //     description: "Lorem ipsum dolor sit amet",
      //     amount: 500,
      //     time: new Date("02-01-2023"),
      //   },
      //   {
      //     id: 3,
      //     title: "Movimiento 4",
      //     description: "Lorem ipsum dolor sit amet",
      //     amount: 200,
      //     time: new Date("02-01-2023"),
      //   },
      //   {
      //     id: 4,
      //     title: "Movimiento 5",
      //     description: "Lorem ipsum dolor sit amet",
      //     amount: -400,
      //     time: new Date("02-01-2023"),
      //   },
      //   {
      //     id: 5,
      //     title: "Movimiento 6",
      //     description: "Lorem ipsum dolor sit amet",
      //     amount: -600,
      //     time: new Date("02-01-2023"),
      //   },
      //   {
      //     id: 6,
      //     title: "Movimiento 7",
      //     description: "Lorem ipsum dolor sit amet",
      //     amount: -300,
      //     time: new Date("02-01-2023"),
      //   },
      //   {
      //     id: 7,
      //     title: "Movimiento 8",
      //     description: "Lorem ipsum dolor sit amet",
      //     amount: 100,
      //     time: new Date("02-01-2023"),
      //   },
      //   {
      //     id: 8,
      //     title: "Movimiento 9",
      //     description: "Lorem ipsum dolor sit amet",
      //     amount: 300,
      //     time: new Date("01-01-2023"),
      //   },
      //   {
      //     id: 9,
      //     title: "Movimiento 10",
      //     description: "Lorem ipsum dolor sit amet",
      //     amount: 500,
      //     time: new Date("01-01-2023"),
      //   },
      // ],
    }
  },
  computed: {
    totalAmount(){
      return this.movements.reduce((suma, m) => suma + m.amount, 0)
    },
    amounts(){
      const lastDays = this.movements.filter(m => {
        const today = new Date();
        const oldDate = today.setDate(today.getDate() - 30)

        return m.time > oldDate
      }).map(m => m.amount)
      
      return lastDays.map((m,i) => {
        const lastMovements = lastDays.slice(0,i+1);
        return lastMovements.reduce((suma, movements) => suma + movements, 0)
      })
      }
  },
  mounted(){
    const movements = JSON.parse(localStorage.getItem("movements"));
    // this.movements = structuredClone(movements)

    if(Array.isArray(this.movements)){
      this.movements = movements.map(m => ({...m, time: new Date(m.time)}))
    }

  },
  methods: {
    create(movement){
      this.movements.push(movement)
      this.save()
    },
    remove(id){
      this.movements = this.movements.filter(m => m.id != id);
      this.save()
    },
    // Almacenamiento
    save(){
      localStorage.setItem("movements", JSON.stringify(this.movements))
    },
    select(el){
      this.amount = el
    }
  }
};
</script>
