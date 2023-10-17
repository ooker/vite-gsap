<script setup>
import { onMounted, ref } from 'vue';
import { gsap } from 'gsap';


const monsters = [
    {
        name: "Oss Tünn",
        mojo: 57,
        unagi: 22,
        img: 'oss-tynn.webp'
    },
    {
        name: "Mika Kakinaki",
        mojo: 16,
        unagi: 77,
        img: 'mika-kakinaki.webp'
    },
    {
        name: "Ello Moos",
        mojo: 85,
        unagi: 92,
        img: 'ello-moos.webp'
    }
];

const updateLock = ref(false);

const monsterName = ref(null);
const monsterImg = ref(null);
const monsterMojo = ref(null);
const monsterUnagi = ref(null);


let monsterIndex = 0;
const activeMonster = ref({
    name: "Nipi Tiri",
    mojo: 0,
    unagi: 0,
    img: ""
});


const clickHandler = () => {
    if(!updateLock.value) {
        updateMonster();
    }
}

const updateMonster = () => {
    updateLock.value = true;

    // name
    gsap.to(monsterName.value, { 
        duration: 0.2, 
        rotateX: -90, 
        z: 250,
        onComplete:()=>{
            activeMonster.value.name = monsters[monsterIndex].name
        } 
    });
    gsap.to(monsterName.value, { 
        duration: 0.5, 
        rotateX: 0,
        z: 30,
        delay:0.3
    });

    // image
    gsap.to(monsterImg.value, { 
        duration: 0.3, 
        scale: 3,
        alpha: 0, 
        onComplete:()=>{
            activeMonster.value.img = monsters[monsterIndex].img
        } 
    });
    gsap.to(monsterImg.value, { 
        duration: 0.5, 
        scale: 1,
        alpha: 1,
        ease: "back.out(1.1)",
        delay:0.3
    });

    // mojo & unagi
    gsap.to(monsterMojo.value, { 
        duration: 1, 
        width: `${activeMonster.value.mojo}%`,
        ease: "back.out(1.3)",
        delay: 0.3
    });
    gsap.to(monsterUnagi.value, { 
        duration: 1, 
        width: `${activeMonster.value.unagi}%`,
        ease: "back.out(1.3)",
        delay: 0.5
    });
    
    setTimeout(updateIndex, 1500);

}

const updateIndex = () => {
    if(monsterIndex === monsters.length-1){
        monsterIndex = 0;
    } else {
        monsterIndex++;
    }
    activeMonster.value.mojo = monsters[monsterIndex].mojo;
    activeMonster.value.unagi = monsters[monsterIndex].unagi;
    updateLock.value = false;
}

onMounted(()=>{
    activeMonster.value.img = monsters[monsterIndex].img;
    activeMonster.value.name = monsters[monsterIndex].name;
    activeMonster.value.mojo = monsters[monsterIndex].mojo;
    activeMonster.value.unagi = monsters[monsterIndex].unagi;

    gsap.set(".monster__name-container", {
        perspective: 300
    });

    updateMonster();
});

</script>






<template>
    <section class="monster" @click="clickHandler">
        <div class="monster__img-container">
            <img ref="monsterImg" class="monster__img" :src="`/monsters/${activeMonster.img}`" />
        </div>
        <div class="monster__name-container">
            <h1 ref="monsterName" class="monster__name">{{ activeMonster.name }}</h1>
        </div>
        
        <h4>MOJO:</h4>
        <div class="scale">
            <div ref="monsterMojo" class="scale__inner"></div>
        </div>
        
        <h4>UNAGI:</h4>
        <div class="scale">
            <div ref="monsterUnagi" class="scale__inner"></div>
        </div>
    </section>

</template>






<style scoped>
h4 {
    font-size: 1rem;
    font-weight: 600;
}
.monster {
    /* background-color: burlywood; */
    display: flex;
    flex-direction: column;
    place-content: center;
    gap: 0.5rem;
    align-items: center;
    width: 600px;
    height: 600px;
    text-align: center;
    background: wheat;
    border-radius: 3rem;
    box-shadow: 0 0.5rem 0.25rem hsl(0 0% 0% / 0.2);
    overflow: hidden;
}
.monster__img {
    width: 20rem;
    height: auto;
}
.monster__img-container {
    width: 20rem;
    height: 20rem;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 50%;

    background-color: var(--vt-c-indigo);
    border: 0.5rem solid var(--vt-c-indigo);
    overflow: hidden;
}
.monster__name {
    font-weight: 900;
}
.scale {
    display: flex;
    justify-content: center;
    width: 20rem;
    height: 0.5rem;
    border: 1px solid black;
    border-radius: 0.25rem;
    background: white;
    overflow: hidden;
}
.scale__inner {
    background-color: brown;
    height: 100%;
}
</style>
