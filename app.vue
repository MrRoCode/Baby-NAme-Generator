<template>
  <div class="box">
    <h1>Baby Name Gerator</h1>
    <p>Choose your option and click "Find Names" button below"</p>
    <div class="options-container">
      <Option 
        v-for="option in optionsArray" 
        :key="option.title" 
        :option="option"
        :options="options"
        />
      <button class="primary" @click="computeSelectedNames">Find Name</button>
    </div>
    <div class="card-container">
       <CardName 
        v-for="(name, index) in selectedNames" 
        :key="name" 
        :name="name" 
        
        @remove="() => removeName(index)" 
        :index="index"
        />
    </div>
  </div>
</template>

<script setup lang="ts">

import { Gender, Popularity, Length, names} from '@/data'
  interface OptionState {
    gender: Gender;
    popularity: Popularity;
    length: Length;
  }

  const options = reactive<OptionState>({
    gender: Gender.GIRL,
    length: Length.ALL,
    popularity: Popularity.TRENDY
  });

  const computeSelectedNames = () => {
    const filterNames = names
    .filter((name)=> name.gender === options.gender)
    .filter((name)=> name.popularity === options.popularity)
    .filter((name)=>{
      if(options.length === Length.ALL) return true
      else return name.length === options.length
    })

    selectedNames.value = filterNames.map(name=> name.name)
  }

  const selectedNames = ref<string[]>([]);

  const removeName = (index: number) => {
    const filteredNames = [...selectedNames.value];
    filteredNames.splice(index, 1);
    selectedNames.value = filteredNames;
  };

  const optionsArray = [
    {
      title: "1) Choose a gender",
      category: "gender",
      buttons: [Gender.GIRL, Gender.BOY, Gender.UNISEX]
    },
    {
      title: "2) Choose the name popularity",
      category: "popularity",
      buttons: [Popularity.TRENDY, Popularity.UNIQUE]
    },
    {
      title: "3) Choose nam's length",
      category: "length",
      buttons: [Length.ALL, Length.LONG, Length.SHORT]
    },
  ]
</script>

<style scoped>
.box {
  font-family: Arial, Helvetica, sans-serif;
  color: rgb(27, 60, 138);
  max-width: 50rem;
  margin: 0 auto;
  text-align: center;
}

h1 {
  font-size: 3rem;
}

.options-container{
  background-color: rgb(255, 238, 236);
  border-radius: 2rem;
  padding: 1rem;
  width: 95%;
  margin: 0 auto;
  margin-top: 4rem;
  position: relative;
}

.primary {
  background-color: rgb(249, 87, 89);
  color: white;
  border: none;
  border-radius: 6.5rem;
  padding: 0.75rem;
  font-size: 1rem;
  margin-top: 1rem;
  cursor: pointer;
}

.card-container {
  display: flex;
  margin-top: 3rem;
  flex-wrap: wrap;
}
</style>