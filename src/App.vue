<script setup>
import { ref, computed } from 'vue';

const slike = ref({
  Jabuka: "https://www.svgrepo.com/show/530203/apple.svg",
  Mrkva: "https://www.svgrepo.com/show/530216/carrot.svg",
  Kruh: "https://www.svgrepo.com/show/530223/bread.svg",
  Sir: "https://www.svgrepo.com/show/530219/cake.svg"
});

const proizvodi = ref([
  { naziv: "Jabuka", cijena: 0.25 },
  { naziv: "Mrkva", cijena: 0.12 },
  { naziv: "Kruh", cijena: 2.00 },
  { naziv: "Sir", cijena: 4.48 }
]);

const korisnik = ref({
  jeAdmin: true,
  osobni_podaci: {
    ime: "Marko",
    prezime: "Krivić",
    adresa: {
      grad: "Pula",
      ulica: "Veruda",
      broj: 32
    },
    broj_telefona: "+091-123-456"
  },
  kosarica: [
    { naziv: "Jabuka", količina: 4 },
    { naziv: "Mrkva", količina: 12 },
    { naziv: "Kruh", količina: 3 },
    { naziv: "Sir", količina: 1 }
  ]
});

const je = ref(korisnik.value.jeAdmin);

const dohvatiCijenu = (naziv) => {
  const proizvod = proizvodi.value.find(p => p.naziv === naziv);
  return proizvod ? proizvod.cijena : 0;
};

const ukupnaCijenaProizvoda = (naziv, količina) => {
  return dohvatiCijenu(naziv) * količina;
};

const sveukupnaCijena = () => {
  const ukupnoJabuka = ukupnaCijenaProizvoda("Jabuka", korisnik.value.kosarica[0].količina);
  const ukupnoMrkva = ukupnaCijenaProizvoda("Mrkva", korisnik.value.kosarica[1].količina);
  const ukupnoKruh = ukupnaCijenaProizvoda("Kruh", korisnik.value.kosarica[2].količina);
  const ukupnoSir = ukupnaCijenaProizvoda("Sir", korisnik.value.kosarica[3].količina);
  return ukupnoJabuka + ukupnoMrkva + ukupnoKruh + ukupnoSir;
};

const najskupljaStavka = computed(() => {
  const ukupneCijene = [
    { naziv: "Jabuka", ukupno: ukupnaCijenaProizvoda("Jabuka", korisnik.value.kosarica[0].količina) },
    { naziv: "Mrkva", ukupno: ukupnaCijenaProizvoda("Mrkva", korisnik.value.kosarica[1].količina) },
    { naziv: "Kruh", ukupno: ukupnaCijenaProizvoda("Kruh", korisnik.value.kosarica[2].količina) },
    { naziv: "Sir", ukupno: ukupnaCijenaProizvoda("Sir", korisnik.value.kosarica[3].količina) }
  ];
  let maxCijena = 0;
  let nazivNajskuplje = '';
  
  for (const stavka of ukupneCijene) {
    if (stavka.ukupno > maxCijena) {
      maxCijena = stavka.ukupno;
      nazivNajskuplje = stavka.naziv;
    }
  }
  return nazivNajskuplje;
});
</script>
  
<template>
  <div class="flex justify-center items-center">
    <div class="mt-20 bg-gray-200 h-40 w-100 rounded-xl border-2 border-gray-500">
  <h4 class="pt-2 pb-2 pl-5 pr-5 text-2xl font-bold" :class="je ? 'text-red-500' : 'text-blue-500'"> Korisnički podatci  <hr class="border-gray-500 pt-2"> </h4> 
      <div class="pl-5 text-lg font-bold" :class="je ? 'text-red-500' : 'text-black-500'">Ime: <span class="font-normal" :class="je ? 'text-red-500' : 'text-black-500'">   {{ korisnik.osobni_podaci.ime }} {{ korisnik.osobni_podaci.prezime }}</span></div> 
      <div class="pl-5 text-lg font-bold" :class="je ? 'text-red-500' : 'text-black-500'">Adresa: <span class="font-normal" :class="je ? 'text-red-500' : 'text-black-500'">{{ korisnik.osobni_podaci.adresa.ulica }} {{ korisnik.osobni_podaci.adresa.broj }}, {{ korisnik.osobni_podaci.adresa.grad }}</span></div>
      <div class="pl-5 text-lg font-bold" :class="je ? 'text-red-500' : 'text-black-500'">Telefon: <span class="font-normal" :class="je ? 'text-red-500' : 'text-black-500'">{{ korisnik.osobni_podaci.broj_telefona }}</span></div>
    </div>
  </div>

  <div class="mt-15 flex justify-center items-center">
    <div class="bg-gray-200 rounded-xl border-2 border-gray-500 p-4 w-100">
      <div class="flex flex-col space-y-2">
        <div class="text-3xl"> Košarica </div>

      <div class="h-20 w-full rounded-lg flex items-center p-4 gap-4" :class="proizvodi[0].naziv === najskupljaStavka ? 'bg-red-200' : 'bg-white'">
        <img :src="slike.Jabuka" class="h-12 w-12" />
          <div class="flex flex-col"><span class="font-bold">{{proizvodi[0].naziv}}</span><span>Cijena: {{proizvodi[0].cijena}} | Kolicina: {{korisnik.kosarica[0].količina}}</span> <span> Ukupno: {{ukupnaCijenaProizvoda("Jabuka", korisnik.kosarica[0].količina) }}</span></div>
        </div>

        <div class="h-20 w-full rounded-lg flex items-center p-4 gap-4" :class="proizvodi[1].naziv === najskupljaStavka ? 'bg-red-200' : 'bg-white'">
          <img :src="slike.Mrkva" class="h-12 w-12" />
          <div class="flex flex-col"><span class="font-bold">{{proizvodi[1].naziv}}</span><span>Cijena: {{proizvodi[1].cijena}} | Kolicina: {{korisnik.kosarica[1].količina}}</span> <span> Ukupno: {{ukupnaCijenaProizvoda("Mrkva", korisnik.kosarica[1].količina) }}</span>
          </div>
        </div>

        <div class="h-20 w-full rounded-lg flex items-center p-4 gap-4" :class="proizvodi[2].naziv === najskupljaStavka ? 'bg-red-200' : 'bg-white'">
          <img :src="slike.Kruh" class="h-12 w-12" />
          <div class="flex flex-col"><span class="font-bold">{{proizvodi[2].naziv}}</span><span>Cijena: {{proizvodi[2].cijena}} | Kolicina: {{korisnik.kosarica[2].količina}}</span> <span> Ukupno: {{ukupnaCijenaProizvoda("Kruh", korisnik.kosarica[2].količina) }}</span>
          </div>
        </div>
        <div class="h-20 w-full rounded-lg flex items-center p-4 gap-4" :class="proizvodi[3].naziv === najskupljaStavka ? 'bg-red-200' : 'bg-white'">
          <img :src="slike.Sir" class="h-12 w-12" />
          <div class="flex flex-col"><span class="font-bold">{{proizvodi[3].naziv}}</span><span>Cijena: {{proizvodi[3].cijena}} | Kolicina: {{korisnik.kosarica[3].količina}}</span> <span> Ukupno: {{ukupnaCijenaProizvoda("Sir", korisnik.kosarica[3].količina)}}</span>
          </div>
        </div>

        <div class="mt-2 text-xl font-bold">
          Ukupna cijena: {{ sveukupnaCijena() }}
        </div>
      </div>
    </div>
  </div>
</template>