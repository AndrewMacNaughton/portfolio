<template>
  <div>
    <tabs-component :items="attractions.slice(0, 10)" @tabChanged="handleTabsChanged">
      <div class="wrapper">
        <h2>{{ selectedAttraction.name }}</h2>
        <div>{{ selectedAttraction.details }}</div>
      </div>
    </tabs-component>
  </div>
</template>

<script setup>
const attractions = ref([
  {
    id: 1,
    name: "Eiffel Tower",
    details:
      "An iconic Parisian landmark, the Eiffel Tower stands 324 meters tall and offers stunning panoramic views of the city from its observation decks. Designed by Gustave Eiffel, it's a global symbol of romance and engineering excellence.",
    selected: false,
  },
  {
    id: 2,
    name: "The Louvre",
    details:
      "Located in Paris, the Louvre is the world's largest art museum, housing over 35,000 works including the Mona Lisa and the Venus de Milo. Its historic glass pyramid entrance blends modern design with classical architecture.",
    selected: true,
  },
  {
    id: 3,
    name: "Statue of Liberty",
    details:
      "A gift from France to the United States, this colossal statue on Liberty Island symbolizes freedom and democracy. Designed by Frédéric Auguste Bartholdi, it stands 93 meters tall and offers views of New York Harbor.",
  },
  {
    id: 4,
    name: "Machu Picchu",
    details:
      "Nestled in the Andes Mountains of Peru, Machu Picchu is an ancient Incan city renowned for its archaeological significance and breathtaking views. It's a UNESCO World Heritage site and a marvel of pre-Columbian engineering.",
  },
  {
    id: 5,
    name: "Great Wall of China",
    details:
      "Stretching over 13,000 miles, the Great Wall of China is an ancient fortification built to protect against invasions. It showcases incredible historical and architectural achievements across varied landscapes.",
  },
  {
    id: 6,
    name: "Taj Mahal",
    details:
      "An exquisite white marble mausoleum in Agra, India, the Taj Mahal is renowned for its stunning symmetry and intricate inlay work. Built by Emperor Shah Jahan in memory of his wife Mumtaz Mahal, it’s a UNESCO World Heritage site.",
  },
  {
    id: 7,
    name: "Pyramids of Giza",
    details:
      "Located on the Giza Plateau near Cairo, Egypt, the Pyramids of Giza include the Great Pyramid, one of the Seven Wonders of the Ancient World. These monumental structures were built as tombs for pharaohs and are a testament to ancient Egyptian engineering.",
  },
  {
    id: 8,
    name: "Colosseum",
    details:
      "This iconic Roman amphitheater, also known as the Flavian Amphitheatre, is a symbol of ancient Rome’s grandeur. It could hold up to 80,000 spectators and was used for gladiatorial contests and public spectacles.",
  },
  {
    id: 9,
    name: "Sagrada Família",
    details:
      "A masterpiece by architect Antoni Gaudí, the Sagrada Família in Barcelona is an extraordinary basilica renowned for its intricate facades and towering spires. Construction began in 1882 and remains ongoing.",
  },
  {
    id: 10,
    name: "Niagara Falls",
    details:
      "A collective name for three waterfalls on the border of the U.S. and Canada, Niagara Falls is famous for its massive water flow and breathtaking views. It attracts millions of visitors annually with its awe-inspiring natural beauty.",
  },
  {
    id: 11,
    name: "Grand Canyon",
    details:
      "Located in Arizona, USA, the Grand Canyon is a vast, visually overwhelming landscape carved by the Colorado River. Spanning 277 miles, it offers dramatic views and is a testament to the natural forces of erosion.",
  },
  {
    id: 12,
    name: "Petra",
    details:
      "An ancient city in Jordan, Petra is renowned for its rock-cut architecture and water conduit system. Known as the 'Rose City' for its pink sandstone cliffs, it was a major trading hub in antiquity and is a UNESCO World Heritage site.",
  },
  {
    id: 13,
    name: "Angkor Wat",
    details:
      "The largest religious monument in the world, Angkor Wat is a stunning temple complex in Cambodia. Originally built as a Hindu temple, it was later transformed into a Buddhist one and is famous for its grand scale and intricate carvings.",
  },
  {
    id: 14,
    name: "Museum of Modern Art (MoMA)",
    details:
      "Located in New York City, MoMA is a leading institution for modern and contemporary art. It houses works by iconic artists like Van Gogh, Warhol, and Picasso, and offers a dynamic program of exhibitions and educational initiatives.",
  },
  {
    id: 15,
    name: "Burj Khalifa",
    details:
      "The tallest building in the world, the Burj Khalifa in Dubai stands at 828 meters. Its sleek design and state-of-the-art engineering offer spectacular views and luxury accommodations, making it a global architectural icon.",
  },
  {
    id: 16,
    name: "Sydney Opera House",
    details:
      "Located on Sydney Harbour, the Sydney Opera House is renowned for its distinctive sail-like design. It hosts a variety of performances and is a symbol of Australia’s vibrant arts scene and architectural innovation.",
  },
  {
    id: 17,
    name: "Golden Gate Bridge",
    details:
      "Spanning the Golden Gate Strait in San Francisco, this iconic red suspension bridge is renowned for its striking design and engineering. Completed in 1937, it connects San Francisco to Marin County and offers stunning views of the bay.",
  },
  {
    id: 18,
    name: "Mount Everest",
    details:
      "The highest peak in the world, Mount Everest stands at 8,848 meters. Located in the Himalayas on the border of Nepal and China, it attracts climbers from around the globe seeking to conquer its challenging slopes.",
  },
  {
    id: 19,
    name: "Venice Canals",
    details:
      "The intricate network of canals in Venice, Italy, is famous for its scenic gondola rides and historic architecture. These waterways serve as the main thoroughfares in a city renowned for its unique charm and romantic atmosphere.",
  },
  {
    id: 20,
    name: "Christ the Redeemer",
    details:
      "This colossal statue of Jesus Christ overlooks Rio de Janeiro from the Corcovado mountain. Standing 30 meters tall, it’s a symbol of peace and one of the New Seven Wonders of the World, offering panoramic views of the city.",
  },
  {
    id: 21,
    name: "Acropolis of Athens",
    details:
      "The Acropolis is an ancient citadel in Athens, Greece, featuring ruins like the Parthenon. It is a symbol of classical Greek civilization and democracy, offering insights into ancient architecture and historical significance.",
  },
  {
    id: 22,
    name: "Forbidden City",
    details:
      "Located in Beijing, China, the Forbidden City is a vast imperial palace complex that served as the home of Chinese emperors for nearly 500 years. It’s renowned for its grand architecture and rich historical artifacts.",
  },
  {
    id: 23,
    name: "Buckingham Palace",
    details:
      "The official residence of the British monarch in London, Buckingham Palace is famous for its grand architecture and the Changing of the Guard ceremony. It’s a symbol of the British monarchy and an iconic landmark.",
  },
  {
    id: 24,
    name: "St. Peter's Basilica",
    details:
      "Situated in Vatican City, St. Peter’s Basilica is one of the world’s largest churches and a major pilgrimage site. Designed by Michelangelo and other renowned architects, it’s a masterpiece of Renaissance architecture.",
  },
  {
    id: 25,
    name: "Museum of Islamic Art",
    details:
      "Located in Doha, Qatar, the Museum of Islamic Art houses one of the world’s most comprehensive collections of Islamic art. Its striking modern architecture complements the diverse and historically significant artifacts inside.",
  },
]);
import { ref, computed } from "vue";
import TabsComponent from "@/components/Tabs/Tabs.vue";
const selectedTab = ref(1);
const handleTabsChanged = (event) => {
  selectedTab.value = event.id;
};
const selectedAttraction = computed(() => {
  return attractions.value.find((item) => item.id === selectedTab.value);
});
</script>

<style lang="scss" scoped>
.tabs-container {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  .tabs-button-div {
    width: 200px;
  }
}
.wrapper {
  padding: 0 40px;
}
</style>
