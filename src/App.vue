<script>
import AppHeader from "@/components/Header.vue";
import Auth from "@/components/Auth.vue";

import { Howl } from "howler";
export default {
  name: "App",
  components: {
    AppHeader,
    Auth,
  },
  data()
  {
    return {
      playing : false,
      seek: "00:00",
      duration: "00:00",
      PlayerProgress: "0%",
      CurrentSurrah:{
        Name: ""
      }
    }
  },
  methods:
  {
    InitializeAudioPlayer(SurrahURL, Name) {
      if(this.sound instanceof Howl)
      {
        this.sound.unload();
      }
          this.sound = new Howl({
            src: [SurrahURL],
          });
          this.CurrentSurrah.Name = Name;
          this.sound.play();
          this.playing = true;
  
  
          this.sound.on("play", () => {
            requestAnimationFrame(this.progress);
          });
        return ;
    },
    ToggleAudio()
    {
      if(!this.sound.playing)
      {
        this.playing = false;
        return;
      }
      if (this.sound.playing())
      {
        this.playing  = false;
        this.sound.pause();
      }
      else
      {
        this.playing = true;
        this.sound.play();
      }
    },
    progress()
    {
      this.seek = this.FormatTime(this.sound.seek());
      this.duration = this.FormatTime(this.sound.duration());
      this.PlayerProgress = `${(this.sound.seek() / this.sound.duration()) * 100}%`
      if(this.sound.playing())
      {
        requestAnimationFrame(this.progress);
      }
    },
    FormatTime(time)
    {
      const Minutes = Math.floor(time/ 60) || 0 ; 
      const Seconds = Math.round((time - Minutes * 60 ) || 0);
      return `${Minutes}:${Seconds < 10 ? '0': ''}${Seconds}`;
    },
    UpdateSeek(event){
      if(!this.sound.playing)
      {
        return;
      }
      // will not represent the cordinate if the document starts to get smaller need to get the relative cordinate of the progress widget
      const {x, width} = event.currentTarget.getBoundingClientRect();
      // Document = 2000 , Timeline = 1000 , ClientX = 1000 , Distance = 500
      
      const ClickX = event.clientX - x;
      const Percentage = ClickX / width;
      const Seconds = this.sound.duration() * Percentage;
      this.sound.seek(Seconds);
      this.sound.once("seek" , this.progress)
   }
  }
   
   
  
};
</script>

<template>
  <app-header />

  <!-- Introduction -->
  <section class="mb-8 py-20 text-white text-center relative">
    <div
      class="absolute inset-0 w-full h-full bg-contain introduction-bg"
      style="background-image: url(assets/img/song-header.png)"
    ></div>

    <div class="container mx-auto">
      
      <div class="text-white main-header-content">
        <h1 class="font-bold text-5xl mb-5">بسم الله الرحمن الرحيم </h1>
        <p class="w-full md:w-8/12 mx-auto">
          
        </p>
      </div>
    </div>

     <img
      class="relative block mx-auto mt-5 -mb-20 w-auto max-w-full"
      src="assets/img/T_ImprovedBannerResizeableV2.png"
    /> 
  </section>

  <!-- Main Content -->
  <section class="container mx-auto">
    <div class="bg-white rounded border border-gray-200 relative flex flex-col">
      <div class="px-6 pt-6 pb-5 font-bold border-b border-gray-200">
        <span class="card-title">سور</span>
        <!-- Icon -->
        <i class="fa fa-headphones-alt float-right text-green-400 text-xl"></i>
      </div>
      <!-- Playlist -->
      <ol id="playlist">
        <li
          class="flex justify-between items-center p-3 pl-6 cursor-pointer transition duration-300 hover:bg-gray-50"
        >
          <div>
            <a href="#" class="font-bold block text-gray-600" v-on:click = "InitializeAudioPlayer('assets/audio/001.mp3','سورة الفاتحة')">سورة الفاتحة</a>
            <span class="text-gray-500 text-sm">مشاري بن راشد العفاسي</span>
          </div>

          <div class="text-gray-600 text-lg">
            <span class="comments">
              <i class="fa fa-comments text-gray-600"></i>
              17373
            </span>
          </div>
        </li>
        <li
          class="flex justify-between items-center p-3 pl-6 cursor-pointer transition duration-300 hover:bg-gray-50"
        >
          <div>
            <a href="#" class="font-bold block text-gray-600" v-on:click = "InitializeAudioPlayer('assets/audio/002.mp3', 'سورةالبقرة')">سورةالبقرة</a>
            <span class="text-gray-500 text-sm">مشاري بن راشد العفاسي</span>
          </div>

          <div class="text-gray-600 text-lg">
            <span class="comments">
              <i class="fa fa-comments text-gray-600"></i>
              673
            </span>
          </div>
        </li>
        <li
          class="flex justify-between items-center p-3 pl-6 cursor-pointer transition duration-300 hover:bg-gray-50"
        >
          <div>
            <a href="#" class="font-bold block text-gray-600" v-on:click = "InitializeAudioPlayer('assets/audio/003.mp3','سورة العمران')">سورة العمران</a>
            <span class="text-gray-500 text-sm">مشاري بن راشد العفاسي</span>
          </div>

          <div class="text-gray-600 text-lg">
            <span class="comments">
              <i class="fa fa-comments text-gray-600"></i>
              1003
            </span>
          </div>
        </li>
        <li
          class="flex justify-between items-center p-3 pl-6 cursor-pointer transition duration-300 hover:bg-gray-50"
        >
          <div>
            <a href="#" class="font-bold block text-gray-600" v-on:click = "InitializeAudioPlayer('assets/audio/004.mp3','سورة النساء')">سورة النساء</a>
            <span class="text-gray-500 text-sm">مشاري بن راشد العفاسي</span>
          </div>

          <div class="text-gray-600 text-lg">
            <span class="comments">
              <i class="fa fa-comments text-gray-600"></i>
              134
            </span>
          </div>
        </li>
        <li
          class="flex justify-between items-center p-3 pl-6 cursor-pointer transition duration-300 hover:bg-gray-50"
        >
          <div>
            <a href="#" class="font-bold block text-gray-600" v-on:click = "InitializeAudioPlayer('assets/audio/005.mp3' , 'سورة المائدة')">سورة المائدة</a>
            <span class="text-gray-500 text-sm">مشاري بن راشد العفاسي</span>
          </div>

          <div class="text-gray-600 text-lg">
            <span class="comments">
              <i class="fa fa-comments text-gray-600"></i>
              5478
            </span>
          </div>
        </li>
        <li
          class="flex justify-between items-center p-3 pl-6 cursor-pointer transition duration-300 hover:bg-gray-50"
        >
          <div>
            <a href="#" class="font-bold block text-gray-600" v-on:click = "InitializeAudioPlayer('assets/audio/006.mp3', 'سورة الأنعام')">سورة الأنعام</a>
            <span class="text-gray-500 text-sm">مشاري بن راشد العفاسي</span>
          </div>

          <div class="text-gray-600 text-lg">
            <span class="comments">
              <i class="fa fa-comments text-gray-600"></i>
              19002
            </span>
          </div>
        </li>
        <li
          class="flex justify-between items-center p-3 pl-6 cursor-pointer transition duration-300 hover:bg-gray-50"
        >
          <div>
            <a href="#" class="font-bold block text-gray-600" v-on:click = "InitializeAudioPlayer('assets/audio/007.mp3' , 'سورة الأعراف')">سورة الأعراف</a>
            <span class="text-gray-500 text-sm">مشاري بن راشد العفاسي</span>
          </div>

          <div class="text-gray-600 text-lg">
            <span class="comments">
              <i class="fa fa-comments text-gray-600"></i>
              1310
            </span>
          </div>
        </li>
        <li
          class="flex justify-between items-center p-3 pl-6 cursor-pointer transition duration-300 hover:bg-gray-50"
        >
          <div>
            <a href="#" class="font-bold block text-gray-600" v-on:click = "InitializeAudioPlayer('assets/audio/008.mp3' , 'سورة الأنفال')">سورة الأنفال</a>
            <span class="text-gray-500 text-sm">مشاري بن راشد العفاسي</span>
          </div>

          <div class="text-gray-600 text-lg">
            <span class="comments">
              <i class="fa fa-comments text-gray-600"></i>
              198
            </span>
          </div>
        </li>
        <li
          class="flex justify-between items-center p-3 pl-6 cursor-pointer transition duration-300 hover:bg-gray-50"
        >
          <div>
            <a href="#" class="font-bold block text-gray-600" v-on:click = "InitializeAudioPlayer('assets/audio/009.mp3' , 'سورة التوبة')">سورة التوبة</a>
            <span class="text-gray-500 text-sm">مشاري بن راشد العفاسي</span>
          </div>

          <div class="text-gray-600 text-lg">
            <span class="comments">
              <i class="fa fa-comments text-gray-600"></i>
              187
            </span>
          </div>
        </li>
        <li
          class="flex justify-between items-center p-3 pl-6 cursor-pointer transition duration-300 hover:bg-gray-50"
        >
          <div>
            <a href="#" class="font-bold block text-gray-600" v-on:click = "InitializeAudioPlayer('assets/audio/010.mp3','سورة يونس')">سورة يونس</a>
            <span class="text-gray-500 text-sm">مشاري بن راشد العفاسي</span>
          </div>

          <div class="text-gray-600 text-lg">
            <span class="comments">
              <i class="fa fa-comments text-gray-600"></i>
              192
            </span>
          </div>
        </li>
        <li
          class="flex justify-between items-center p-3 pl-6 cursor-pointer transition duration-300 hover:bg-gray-50"
        >
          <div>
            <a href="#" class="font-bold block text-gray-600" v-on:click = "InitializeAudioPlayer('assets/audio/011.mp3' , 'سورة هود')">سورة هود</a>
            <span class="text-gray-500 text-sm">مشاري بن راشد العفاسي</span>
          </div>

          <div class="text-gray-600 text-lg">
            <span class="comments">
              <i class="fa fa-comments text-gray-600"></i>
              981
            </span>
          </div>
        </li>
        <li
          class="flex justify-between items-center p-3 pl-6 cursor-pointer transition duration-300 hover:bg-gray-50"
        >
          <div>
            <a href="#" class="font-bold block text-gray-600" v-on:click = "InitializeAudioPlayer('assets/audio/012.mp3' , 'سورة  يوسف')">سورة  يوسف</a>
            <span class="text-gray-500 text-sm">مشاري بن راشد العفاسي</span>
          </div>

          <div class="text-gray-600 text-lg">
            <span class="comments">
              <i class="fa fa-comments text-gray-600"></i>
              49
            </span>
          </div>
        </li>
        <li
          class="flex justify-between items-center p-3 pl-6 cursor-pointer transition duration-300 hover:bg-gray-50"
        >
          <div>
            <a href="#" class="font-bold block text-gray-600" v-on:click = "InitializeAudioPlayer('assets/audio/013.mp3' , 'سورة الرعد')">سورة الرعد</a>
            <span class="text-gray-500 text-sm">مشاري بن راشد العفاسي</span>
          </div>

          <div class="text-gray-600 text-lg">
            <span class="comments">
              <i class="fa fa-comments text-gray-600"></i>
              15
            </span>
          </div>
        </li>
      </ol>
      <!-- .. end Playlist -->
    </div>
  </section>

  <!-- Player -->
  <div class="fixed bottom-0 left-0 bg-white px-4 py-2 w-full">
    <!-- Track Info -->
    <div class="text-center" v-if="CurrentSurrah.Name">
      <span class="song-title font-bold">{{ CurrentSurrah.Name }}</span> <hr>
      <span class="song-artist">مشاري بن راشد العفاسي</span>
    </div>
    <div class="flex flex-nowrap gap-4 items-center">
      <!-- Play/Pause Button -->
      <button v-on:click.prevent=" ToggleAudio();" type="button">
        <i class="fa  text-gray-500 text-xl" v-bind:class = "{'fa-play': !playing, 'fa-pause' : playing}"></i>
      </button>
      <!-- Current Position -->
      <div class="player-currenttime">{{ seek }}</div>
      <!-- Scrub Container  -->
      <div v-on:click = "UpdateSeek" class="w-full h-2 rounded bg-gray-200 relative cursor-pointer">
        <!-- Player Ball -->
        <span
          class="absolute -top-2.5 -ml-2.5 text-gray-800 text-lg"
          v-bind:style="{left:PlayerProgress}"
        >
          <i class="fas fa-circle"></i>
        </span>
        <!-- Player Progress Bar-->
        <span
          class="block h-2 rounded bg-gradient-to-r from-green-500 to-green-400"
          v-bind:style="{width:PlayerProgress}"
        ></span>
      </div>
      <!-- Duration -->
      <div class="player-duration">{{ duration }}</div>
    </div>
  </div>

  <auth />
</template>

