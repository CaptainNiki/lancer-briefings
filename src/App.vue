<template>
  <Header :header="this.header" />
  <div class="content-container">
    <section class="section-container" id="missions" style="width:435px; height:714px;">
      <div class="section-header clipped-medium-backward">
        <img src="/icons/mission-icon.svg" />
        <h1>Mission Log</h1>
      </div>
      <div class="section-content-container">
        <h3>Current Assignment</h3>
        <Markdown :source="current_md" class="markdown" />
        <h3>Mission List</h3>
        <div class="mission-list-container">
          <Mission
            v-for="item in this.missions"
            :key="item.slug"
            :mission="item"
            :selected="this.mission_slug"
            @click="selectMission(item)"
          />
        </div>
      </div>
    </section>
    <section class="section-container" id="events" style="width:435px; height:714px;">
      <div class="section-header clipped-medium-backward">
        <img src="/icons/events-icon.svg" />
        <h1>Events Log</h1>
      </div>
      <div class="section-content-container">
        <Markdown :source="events" class="markdown" />
      </div>
    </section>
    <section class="section-container" id="pilots" style="width:894px; height:714px;">
      <div style="height:52px; overflow:hidden;">
        <div class="section-header clipped-medium-backward-pilot">
          <img src="/icons/pilot-icon.svg" />
          <h1>Pilot Roster</h1>
        </div>
        <div class="rhombus-back">&nbsp;</div>
      </div>
      <div class="section-content-container">
        <div class="pilot-list-container">
          <Pilot v-for="item in this.pilots" :key="item.slug" :pilot="item" />
        </div>
      </div>
    </section>
  </div>
  <svg
    style="visibility: hidden; position: absolute;"
    width="0"
    height="0"
    xmlns="http://www.w3.org/2000/svg"
    version="1.1"
  >
    <defs>
      <filter id="round">
        <feGaussianBlur in="SourceGraphic" stdDeviation="5" result="blur" />
        <feColorMatrix
          in="blur"
          mode="matrix"
          values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 19 -5"
          result="goo"
        />
        <feComposite in="SourceGraphic" in2="goo" operator="atop" />
      </filter>
    </defs>
  </svg>
  <audio autoplay>
    <source src="/startup.ogg" type="audio/ogg" />
  </audio>
  <Footer/>
</template>

<script>
import Header from './components/layout/Header.vue';
import Footer from './components/layout/Footer.vue';
import Mission from './components/Mission.vue';
import Pilot from './components/Pilot.vue';
import Markdown from 'vue3-markdown-it';
export default {
  components: {
    Header,
    Footer,
    Mission,
    Pilot,
    Markdown
  },
  data() {
    return {
      "mission_slug": "006-1",
      "current_md": "006-1",
      "events": "006",
      "missions": [
         {
          "slug": "006-2",
          "name": "Winds of Dust",
          "status": "available"
        },
        {
          "slug": "005",
          "name": "Oh Ye Little of Faith",
          "status": "available"
        },
        {
          "slug": "005",
          "name": "A Pirate's Life for Me",
          "status": "success"
        },
        {
          "slug": "004",
          "name": "CEZ Reconnaissance",
          "status": "success"
        },
        {
          "slug": "003-2",
          "name": "Emergency Repairs",
          "status": "success"
        },
        {
          "slug": "003-1",
          "name": "Investigate Sabotage",
          "status": "success"
        },
        {
          "slug": "002b",
          "name": "Exterminate the Jagiellons",
          "status": "success"
        },
        {
          "slug": "002a",
          "name": "Traverse to Asteria-03",
          "status": "success"
        },
        {
          "slug": "001",
          "name": "Escape Asteria-09",
          "status": "success"
        },
      ],
      "pilots": [
        {
          "callsign": "Platoon",
          "alias": "Ellie Doyle",
          "code": "462370be-bd0f-41c2-b667-cc75f3a59a96///LCI-DEPLOYMENT///377308ad-ba23-410b-ae37-68a1fb5f8db4",
          "corpro": "IPS-N",
          "frame": "Bligh",
          "mech": "FACTORY"
        },
        {
          "callsign": "Rook",
          "alias": "Hachiko",
          "code": "7cd700cc-c990-48ed-892f-e5468de724c4///LCI-DEPLOYMENT///a98c3e28-ad4a-4f89-bcd9-501464e960da",
          "corpro": "GMS",
          "frame": "Sagarmatha",
          "mech": "FLYING CASTLE"
        },
        {
          "callsign": "Chips",
          "alias": "KF-7640",
          "code": "98ca9616-044e-4f87-b89b-aae4eb3387ec///LCI-DEPLOYMENT///6f572259-6946-41bf-931a-e0543709e892",
          "corpro": "HORUS",
          "frame": "Lycan",
          "mech": "TANTRUM"
        },
        {
          "callsign": "Winner",
          "alias": "Wynne Wigstan",
          "code": "98ca9616-044e-4f87-b89b-aae4eb3387ec///LCI-DEPLOYMENT///6f572259-6123-41bf-931a-akhsdb109e852",
          "corpro": "GMS",
          "frame": "Sagarmatha",
          "mech": "EXCELSIOR"
        },
        {
          "callsign": "I-DOL",
          "alias": "Lia",
          "code": "98ca9616-044e-4f87-b89b-aae4eb3387ec///LCI-DEPLOYMENT///78j22874-6564-asud-hasu-12093712kadjh",
          "corpro": "HORUS",
          "frame": "Goblin",
          "mech": "GORBY"
        },
      ],
      "header": {
        "planet": "ASTERIA-03",
        "year": "503 CE",
        "system": "HESPERIAN SYSTEM",
        "gate": "-",
        "ring": "Beyond the Annamite-Line",
        "headerTitle": "RLC",
        "headerSubtitle": "NAUTILUS",
        "subheaderTitle": "Royal Lancer Corporation",
        "subheaderSubtitle": "RLC",
      },
      "options":{
        "eventsMarkdownPerMission": true
      }
    }
  },
  created() {
    this.loadMissionMarkdown()
    this.loadEventsMarkdown()
  },
  computed: {
  },
  methods: {
    selectMission(mission) {
      this.mission_slug = mission.slug;
      this.loadMissionMarkdown()
      if(this.options.eventsMarkdownPerMission){
        this.loadEventsMarkdown();
      }
    },
    loadMissionMarkdown() {
      let self = this;
      let md = `/missions/${self.mission_slug}.md`
      var client = new XMLHttpRequest();
      client.open('GET', md);
      client.onreadystatechange = function () {
        self.current_md = client.responseText;
      }
      client.send();
    },
    loadEventsMarkdown() {
      let self = this;
      let md = "";
      if(self.options.eventsMarkdownPerMission){
        md = `/events/${self.mission_slug}.md`
      }
      else {
        md = "/events.md"
      }
      var client = new XMLHttpRequest();
      client.open('GET', md);
      client.onreadystatechange = function () {
        self.events = client.responseText;
      }
      client.send();
    }
  }
}
</script>


<style lang="scss">
#app {
  width: 1920px;
  height: 1080px;
  overflow: hidden;
}
</style>
