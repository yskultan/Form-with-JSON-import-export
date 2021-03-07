<template>
  <div class="bg-white w-full sm:px-0 md:px-16 lg:px-48 sm:py-0 md:py-4 lg:py-8">
    <div class="h-full mx-auto sm:p-2 md:p-4 lg:p-16">

    <!-- home block -->

      <div class="part p-6 w-full mb-8">
        <span class="title mb-8">Main</span>
        <div class="w-full flex">
          <label class="subtitle w-1/12">
              Photo
          </label>
          <label class="subtitle w-11/12 ml-20">
              Institute name
          </label>
        </div>
        <div class="flex w-full mt-2 h-36">
            <div class="rounded-lg mr-2 w-1/12 h-24 logo">
              <img :src="bg.data" onerror="this.src='https://www.aphroditebeach.gr/wp-content/uploads/2019/01/white.png'" 
                 class="object-cover rounded-lg h-24" />
            </div>
            <input type="file" ref="bg" class="hidden" @change="loadImg(0, 0,  $event)">
            <button v-show="!bg.loaded"
                    @click="openBg" 
                    class="btn-sm bg-gray-600 py-10 px-4 mr-2">
              <svg class="w-4 h-4 fill-current" viewBox="0 0 20 20"><path d="M10 3a1 1 0 011 1v5h5a1 1 0 110 2h-5v5a1 1 0 11-2 0v-5H4a1 1 0 110-2h5V4a1 1 0 011-1z" clip-rule="evenodd" fill-rule="evenodd"></path></svg>
            </button>
            <button v-show="bg.loaded"
                    @click="unloadImg(institute.home, 0)"
                    class="btn-sm bg-red-500 py-10 px-4 mr-2">
              <svg class="icon w-4 h-4 fill-current" viewBox="0 0 20 20"><path d="M10 3a1 1 0 011 1v5h5a1 1 0 110 2h-5v5a1 1 0 11-2 0v-5H4a1 1 0 110-2h5V4a1 1 0 011-1z" clip-rule="evenodd" fill-rule="evenodd"></path></svg>
            </button>
        <div class="ml-4 w-11/12">
          <input v-model="institute.home.name"
                type="text"
                class="field px-4 py-2 w-full" 
                placeholder="Full"/>
          <input v-model="institute.home.shortname"
                type="text"
                class="field px-4 py-2 mt-2 w-full" 
                placeholder="Short"/>
        </div>
        </div>
        <label class="subtitle mt-8 mb-2">
              Social links
        </label>
        <div v-for="(item, index) in institute.home.social"
            :key="`social-${index}`"
            class="flex mt-2"
          >
          <select v-model="item.platform" class="field w-1/3 py-2 px-4">
              <option value="instagram" selected>Instagram</option>
              <option value="youtube">Youtube</option>
              <option value="vk">VK</option>
              <option value="facebook">Facebook</option>
          </select>
          <input v-model="item.link" 
                 type="url" 
                 pattern="https://.*"
                 placeholder="Link" 
                 class="field ml-2 px-4 py-2 w-2/3" />
        </div>
        <button @click="addField(institute.home.social)" 
                class="btn-md bg-gray-600 py-1 px-8 mt-3">
                Another item
        </button>
        <button v-show="institute.home.social.length > 1" 
                @click="removeField(institute.home.social.length--, institute.home.social)" 
                class="btn-md bg-red-500 py-1 px-8 ml-2 mt-3">
                Delete item
        </button>
      </div>

    <!-- about block -->

      <div class="part p-6 w-full mb-8">
        <span class="title mb-8">About</span>
        <label class="subtitle mb-2">
              Institute description
        </label>
        <textarea v-model="institute.about" class="field px-4 py-2 mt-2 w-full h-40" />
        <span class="text-gray-600 pt-1 block">
          Word count should be between 100 and 120
        </span>
      </div>

    <!-- numbers block -->

      <div class="part p-6 w-full mb-8">
        <span class="title mb-8">Our numbers</span>
        <label class="subtitle mb-2">
          Important numbers
        </label>
        <div v-for="(item, index) in institute.numbers" :key="`numbers-${index}`" class="flex mt-2">
            <input v-model="item.number"
                   class="field px-4 py-2 w-1/2" 
                   type="number" 
                   placeholder="Number" />
            <input v-model="item.text"
                   class="field px-4 py-2 ml-4 w-1/2" 
                   type="text" 
                   placeholder="Text" />
        </div>
        <span class="text-gray-600 pt-1 block">
          Minimum three numbers
        </span>
        <button v-show="institute.numbers.length < 6" 
                @click="addField(institute.numbers)" 
                class="btn-md bg-gray-600 py-1 px-8 mr-2 mt-3">
                Another item
        </button>
        <button v-show="institute.numbers.length > 1" 
                @click="removeField(institute.numbers.length--, institute.numbers)" 
                class="btn-md bg-red-500 py-1 px-8 mt-3">
                Delete item
        </button>
      </div>

    <!-- education block -->

      <div class="part p-6 w-full mb-8">
        <span class="title mb-8">Education</span>
        <label class="subtitle mb-2">Levels of education</label>
          <div class="flex mt-4">
          <label class="flex mr-4">
            <div class="bg-white border-2 rounded border-gray-400 w-6 h-6 flex flex-shrink-0 justify-center items-center mr-2 focus-within:border-blue-500">
              <input type="checkbox" v-model="institute.education.categories" value="bachelor" class="opacity-0 absolute">
              <svg class="fill-current hidden w-4 h-4 text-orange-500 pointer-events-none" viewBox="0 0 20 20"><path d="M0 11l2-2 5 5L18 3l2 2L7 18z"/></svg>
            </div>
            <div class="select-none text-gray-600">bachelor</div>
          </label>
          <label class="flex mr-4">
            <div class="bg-white border-2 rounded border-gray-400 w-6 h-6 flex flex-shrink-0 justify-center items-center mr-2 focus-within:border-blue-500">
              <input type="checkbox" v-model="institute.education.categories" value="specialist" class="opacity-0 absolute">
              <svg class="fill-current hidden w-4 h-4 text-orange-500 pointer-events-none" viewBox="0 0 20 20"><path d="M0 11l2-2 5 5L18 3l2 2L7 18z"/></svg>
            </div>
            <div class="select-none text-gray-600">specialist</div>
          </label>
          <label class="flex mr-4">
            <div class="bg-white border-2 rounded border-gray-400 w-6 h-6 flex flex-shrink-0 justify-center items-center mr-2 focus-within:border-blue-500">
              <input type="checkbox" v-model="institute.education.categories" value="master" class="opacity-0 absolute">
              <svg class="fill-current hidden w-4 h-4 text-orange-500 pointer-events-none" viewBox="0 0 20 20"><path d="M0 11l2-2 5 5L18 3l2 2L7 18z"/></svg>
            </div>
            <div class="select-none text-gray-600">master</div>
          </label>
        </div>
        <label class="subtitle mt-8">Programs</label>
        <div
            v-for="(category, index) in institute.education.categories"
            :key="`categories-${index}`"
            class="mt-8"
          >
          <label class="font-semibold text-gray-600 block mt-2">{{ category }}</label>
          <div
              v-for="(item, index) in institute.education.programs[category]"
              :key="`programs-${index}`"
              class="mt-4"
            >
            <label class="font-semibold text-gray-600 block mt-2 pb-1">{{ index + 1 }}</label>
              <input v-model="item.title"  
                      type="text" 
                      placeholder="Title" 
                      class="field px-4 py-2 mr-2 w-full"/>
            <textarea v-model="item.description"
                      class="field px-4 py-2 mt-2 w-full h-40" 
                      placeholder="Description" />
            <div class="flex w-full">
              <input v-model="item.tuition"  
                      type="text" 
                      placeholder="Tuition" 
                      class="field px-4 py-2 mr-2 w-2/3"/>
              <input v-model="item.years"  
                      type="text" 
                      placeholder="Years" 
                      class="field px-4 py-2 w-1/3"/>
            </div>
            <textarea v-model="item.skills"
                      class="field px-4 py-2 mt-2 w-full h-40" 
                      placeholder="Skills" />
            <span class="text-gray-600 pt-1 block">
              Semicolon separated
            </span>
            <div class="flex w-full mt-2">
              <input v-model="item.file"
                      type="text" 
                      placeholder="File" 
                      class="field px-4 py-2 w-4/5"/>
              <input type="file" :ref="category" class="hidden" @change="loadPdf(category, index, $event)">
              <button v-show="!programs[category][index].loaded" 
                      @click="openPdf(category, index)" 
                      class="btn-md bg-gray-600 py-4 px-32 ml-2">
                <svg class="w-4 h-4 fill-current" viewBox="0 0 20 20"><path d="M10 3a1 1 0 011 1v5h5a1 1 0 110 2h-5v5a1 1 0 11-2 0v-5H4a1 1 0 110-2h5V4a1 1 0 011-1z" clip-rule="evenodd" fill-rule="evenodd"></path></svg>
              </button>
              <button v-show="programs[category][index].loaded" 
                      @click="unloadPdf(category, index)" 
                      class="btn-md bg-red-500 py-4 px-32 ml-2">
                <svg class="w-4 h-4 fill-current icon" viewBox="0 0 20 20"><path d="M10 3a1 1 0 011 1v5h5a1 1 0 110 2h-5v5a1 1 0 11-2 0v-5H4a1 1 0 110-2h5V4a1 1 0 011-1z" clip-rule="evenodd" fill-rule="evenodd"></path></svg>
              </button>
            </div>
          </div>
          <button @click="addField(category)" 
                  class="btn-md bg-gray-600 py-1 px-8 mt-3">
            Another item
          </button>
          <button v-show="institute.education.programs[category].length > 1" 
                  @click="removeField(institute.education.programs[category].length--, category)" 
                  class="btn-md bg-red-500 py-1 px-8 ml-2 mt-3">
            Delete item
          </button>
        </div>
      </div>

    <!-- advantages block -->

      <div class="part p-6 w-full mb-8">
        <span class="title mb-8">Why you should join us</span>
        <label class="subtitle mb-2">
          Advantages
        </label>
        <div v-for="(item, index) in institute.advantages" :key="`advantages-${index}`" class="mt-2">
            <label class="font-semibold text-gray-600 block mt-2 pb-1">{{ index + 1 }}</label>
            <input v-model="item.title"
                   class="field px-4 py-2 w-full" 
                   type="text" 
                   placeholder="Title" />
            <textarea v-model="item.content"
                      placeholder="Description" 
                      class="field px-4 py-2 mt-2 w-full h-40" />
        </div>
        <span class="text-gray-600 pt-1 block">
          First letters should compose a word or a pattern
        </span>
        <button v-show="institute.advantages.length < 6" 
                @click="addField(institute.advantages)" 
                class="btn-md bg-gray-600 py-1 px-8 mr-2 mt-3">
                Another item
        </button>
        <button v-show="institute.advantages.length > 1" 
                @click="removeField(institute.advantages.length--, institute.advantages)" 
                class="btn-md bg-red-500 py-1 px-8 mt-3">
                Delete item
        </button>
      </div>

    <!-- welcome lock -->

      <div class="part p-6 w-full mb-8">
        <span class="title mb-8">Welcome</span>
        <label class="subtitle mb-2">
          International students
        </label>
        <div v-for="(item, index) in institute.welcome" :key="`welcome-${index}`" class="flex mt-2">
            <input v-model="item.quantity"
                   class="field px-4 py-2 w-1/3" 
                   type="number" 
                   placeholder="Quantity" />
            <input v-model="item.nationality"
                   class="field ml-2 px-4 py-2 w-2/3" 
                   type="text" 
                   placeholder="Nationality" />
        </div>
        <button v-show="institute.welcome.length < 4" 
                @click="addField(institute.welcome)" 
                class="btn-md bg-gray-600 py-1 px-8 mr-2 mt-3">
                Another item
        </button>
        <button v-show="institute.welcome.length > 1" 
                @click="removeField(institute.welcome.length--, institute.welcome)" 
                class="btn-md bg-red-500 py-1 px-8 mt-3">
                Delete item
        </button>
      </div>

    <!-- internship block -->

      <div class="part p-6 w-full mb-8">
        <span class="title mb-8">Internship</span>
        <label class="subtitle mb-2">
          Logo
        </label>
        <div v-for="(company, index) in institute.internship" :key="`companies-${index}`" class="flex w-full mt-2">
            <img :loaded="companies[index].loaded" :src="companies[index].data" onerror="this.src='https://www.aphroditebeach.gr/wp-content/uploads/2019/01/white.png'" 
                 class="rounded-full mr-2 w-12 h-12 logo" />
            <input type="file" ref="image" class="hidden" @change="loadImg(1, index, $event)">
            <button v-show="!companies[index].loaded" 
                    @click="openImg(index)" 
                    class="btn-sm bg-gray-600 py-4 px-4 mr-2">
              <svg class="w-4 h-4 fill-current" viewBox="0 0 20 20"><path d="M10 3a1 1 0 011 1v5h5a1 1 0 110 2h-5v5a1 1 0 11-2 0v-5H4a1 1 0 110-2h5V4a1 1 0 011-1z" clip-rule="evenodd" fill-rule="evenodd"></path></svg>
            </button>
            <button v-show="companies[index].loaded" 
                    @click="unloadImg(institute.internship, index)"
                    class="btn-sm bg-red-500 py-4 px-4 mr-2">
              <svg class="icon w-4 h-4 fill-current" viewBox="0 0 20 20"><path d="M10 3a1 1 0 011 1v5h5a1 1 0 110 2h-5v5a1 1 0 11-2 0v-5H4a1 1 0 110-2h5V4a1 1 0 011-1z" clip-rule="evenodd" fill-rule="evenodd"></path></svg>
            </button>
            <input v-model="company.name"
                   class="field ml-8 px-4 py-2 w-full" 
                   type="text" 
                   placeholder="Name" />
        </div>
        <button v-show="institute.internship.length < 5" 
                @click="addField(institute.internship)" 
                class="btn-md bg-gray-600 py-1 px-8 mr-2 mt-3">
                Another item
        </button>
        <button v-show="institute.internship.length > 1" 
                @click="removeField(institute.internship.length--, institute.internship)" 
                class="btn-md bg-red-500 py-1 px-8 mt-3">
                Delete item
        </button>
      </div>

    <!-- reviews block -->

      <div class="part p-6 w-full mb-8">
        <span class="title mb-8">Reviews</span>
        <label class="subtitle mb-2">
          Photo
        </label>
        <div v-for="(review, index) in institute.reviews" :key="`reviews-${index}`" class="w-full mt-2">
            <div class="flex w-full">
              <input v-model="review.name"  
                      type="text" 
                      placeholder="Name" 
                      class="field px-4 py-2 mr-2 w-2/3"/>
              <input v-model="review.role"  
                      type="text" 
                      placeholder="Role" 
                      class="field px-4 py-2 w-1/3"/>
            </div>
            <div class="flex w-full mt-4">
            <div class="rounded-lg mr-2 w-32 logo">
              <img :src="reviews[index].data" onerror="this.src='https://www.aphroditebeach.gr/wp-content/uploads/2019/01/white.png'" 
                 class="object-cover rounded-lg h-40" />
            </div>
            <input type="file" ref="photo" class="hidden" @change="loadImg(2, index, $event)">
            <button v-show="!reviews[index].loaded" 
                    @click="openPhoto(index)" 
                    class="btn-sm bg-gray-600 py-20 px-4 mr-2">
              <svg class="w-4 h-4 fill-current" viewBox="0 0 20 20"><path d="M10 3a1 1 0 011 1v5h5a1 1 0 110 2h-5v5a1 1 0 11-2 0v-5H4a1 1 0 110-2h5V4a1 1 0 011-1z" clip-rule="evenodd" fill-rule="evenodd"></path></svg>
            </button>
            <button v-show="reviews[index].loaded" 
                    @click="unloadImg(institute.reviews, index)"
                    class="btn-sm bg-red-500 py-20 px-4 mr-2">
              <svg class="icon w-4 h-4 fill-current" viewBox="0 0 20 20"><path d="M10 3a1 1 0 011 1v5h5a1 1 0 110 2h-5v5a1 1 0 11-2 0v-5H4a1 1 0 110-2h5V4a1 1 0 011-1z" clip-rule="evenodd" fill-rule="evenodd"></path></svg>
            </button>
            <textarea v-model="review.text"
                      class="field px-4 py-2 w-full h-40" 
                      type="text" 
                      placeholder="Review" />
            </div>
        </div>
        <span class="text-gray-600 pt-1 block">
          Review word count should be between 50 and 70
        </span>
        <button v-show="institute.reviews.length < 5" 
                @click="addField(institute.reviews)" 
                class="btn-md bg-gray-600 py-1 px-8 mr-2 mt-3">
                Another item
        </button>
        <button v-show="institute.reviews.length > 1" 
                @click="removeField(institute.reviews.length--, institute.reviews)" 
                class="btn-md bg-red-500 py-1 px-8 mt-3">
                Delete item
        </button>
      </div>

    <!-- buttons block -->
    
      <div class="flex w-full justify-between">
        <div class="flex">
        <button @click="exportZip" 
                class="bg-orange-600 btn-lg py-3 px-6 mt-3">
            Save
        </button>
        <button class="bg-gray-600 ml-4 btn-lg py-3 px-6 mt-3" 
                @click="openFile">
            Open
        </button>
        </div>
        <button class="bg-red-500 ml-4 btn-lg py-3 px-6 mt-3" 
                @click="resetAll">
            Reset
        </button>
        <input type="file" ref="file" class="hidden" @change="importZip">
      </div>
    </div>
  </div>
</template>

<script>
import JSZip from 'jszip'
import JSZipUtils from 'jszip-utils'
import { saveAs } from 'file-saver'

export default {
  name: 'SibForm',
  data() {
    return {
      institute: {
        home: {
          name: "",
          shortname: "",
          bg: "",
          social: [{ platform:"instagram", link: "" }],
        },
        about: "",
        numbers: [{ number: "", text: "" }],
        education: {
          categories: ["bachelor"],
          programs: {
            bachelor: [{ title: "", skills: "", description: "", years: "", tuition: "", file: "" }],
            specialist: [{ title: "", skills: "", description: "", years: "", tuition: "", file: "" }],
            master: [{ title: "", skills: "", description: "", years: "", tuition: "", file: "" }]
          }
        },
        advantages: [{ title: "", content: "" }],
        welcome: [{ quantity: "", nationality: "" }],
        internship: [{ name: "", logo: ""}],
        reviews: [{ name:"", role:"", photo: "", review: ""}]
      },
      programs: { bachelor: [{ loaded: false, name: "", data: "" }], specialist: [{ loaded: false, name: "", data: "" }], master: [{ loaded: false, name: "", data: "" }]},
      companies: [{ loaded: false, name: "", data: "" }],
      reviews: [{ loaded: false, name: "", data: "" }],
      bg: { loaded: false, name: "", data: "" },
      defaultData: {}
    };
  },
  created() {
    this.defaultData = JSON.parse(JSON.stringify(this.$data))
  },
  methods: {
    addField(item) {
      if (item === this.institute.home.social) {
        item.push({ platform: "instagram",  link: "" })
        return
      } 
      if (item === this.institute.internship) {
        this.companies.push({ loaded: false, name: "", data: "" });
        item.push({ name: "", logo: ""});
        return
      }
      if (item === this.institute.reviews) {
        this.reviews.push({ loaded: false, name: "", data: "" });
        item.push({ name:"", role:"", photo: "", review: ""});
        return
      }
      if (this.institute.education.categories.includes(item)) {
        this.institute.education.programs[item].push({ title: "", skills: "", description: "", years: "", tuition: "", file: "" });
        this.programs[item].push({ loaded: false, name: "", data: "" });
        return
      }
      else {
        item.push({});
        return
      }
    },
    removeField(index, item) {
      if (item === this.institute.internship) {
        this.companies.splice(index, 1);
        item.splice(index, 1);
      }
      if (item === this.institute.reviews) {
        this.reviews.splice(index, 1);
        item.splice(index, 1);
      }
      if (this.institute.education.categories.includes(item)) {
        this.institute.education.programs[item].splice(index, 1);
        this.programs[item].splice(index, 1);
      }
      else
        item.splice(index, 1);
    },
    loadImg(item, index, event) {
      const img = event.target.files[0]
      console.log(item)
      if (item === 1) {
        let filename = "company-" + index + ".png"
        this.institute.internship[index].logo = filename
        this.companies[index] = {
          loaded: true,
          name: filename,
          data: URL.createObjectURL(img)
        }
        this.$refs.image[index].value = ""
        this.$forceUpdate();
        return
      }
      if (item === 2) {
        let filename = "review-" + index + ".png"
        this.institute.reviews[index].photo = filename
        this.reviews[index] = {
          loaded: true,
          name: filename,
          data: URL.createObjectURL(img)
        }
        this.$refs.photo[index].value = ""
        this.$forceUpdate();
        return
      }
      if (item === 0) {
        this.institute.home.bg = "bg.png"
        this.bg = {
          loaded: true,
          name: "bg.png",
          data: URL.createObjectURL(img)
        }
        this.$refs.bg.value = ""
        this.$forceUpdate();
        return
      }
    },
    loadPdf(category, index, event) {
      const pdf = event.target.files[0]
      if (category === "bachelor") {
        let filename = "bac-" + index + ".pdf"
        this.institute.education.programs[category][index].file = filename
        this.programs[category][index] = {
          loaded: true,
          name: filename,
          data: URL.createObjectURL(pdf)
        }
        this.$refs[category][index].value = ""
        this.$forceUpdate();
        return
      }
      if (category === "specialist") {
        let filename = "spec-" + index + ".pdf"
        this.institute.education.programs[category][index].file = filename
        this.programs[category][index] = {
          loaded: true,
          name: filename,
          data: URL.createObjectURL(pdf)
        }
        this.$refs[category][index].value = ""
        this.$forceUpdate();
        return
      }
      if (category === "master") {
        let filename = "mas-" + index + ".pdf"
        this.institute.education.programs[category][index].file = filename
        this.programs[category][index] = {
          loaded: true,
          name: filename,
          data: URL.createObjectURL(pdf)
        }
        this.$refs[category][index].value = ""
        this.$forceUpdate();
        return
      }
    },
    unloadImg(item, index) {
      if (item === this.institute.internship) {
        this.institute.internship[index].logo = ""
        this.companies[index].loaded = false
        this.companies[index].data = ""
        this.$forceUpdate();
      } 
      if (item === this.institute.reviews) {
        this.institute.reviews[index].photo = ""
        this.reviews[index].loaded = false
        this.reviews[index].data = ""
        this.$forceUpdate();
      } 
      else {
        this.institute.home.bg = ""
        this.bg.loaded = false
        this.bg.data = ""
        this.$forceUpdate();
      }
    },
    unloadPdf(category, index) {
      this.institute.education.programs[category][index].file = ""
      this.programs[category][index].loaded = false
      this.programs[category][index].data = ""
      this.$forceUpdate();
    },
    resetLoaded() {
      this.companies.pop()
      this.reviews.pop()
      this.programs.bachelor.pop()
      this.programs.master.pop()
      this.programs.specialist.pop()
    },
    resetAll() {
      this.institute = this.defaultData.institute
      this.programs = this.defaultData.programs
      this.reviews = this.defaultData.reviews
      this.companies = this.defaultData.companies
      this.bg = this.defaultData.bg
    },
    openBg(){
      this.$refs.bg.click()
    },
    openImg(index){
      this.$refs.image[index].click()
    },
    openPhoto(index){
      this.$refs.photo[index].click()
    },
    openPdf(category, index) {
      this.$refs[category][index].click()
      console.log("opened", category)
    },
    openFile(){
      this.$refs.file.click();
    },
    exportZip() {
      const imgList = []
      const expInstitute = JSON.parse(JSON.stringify(this.$data));
      
      imgList.push({name: expInstitute.bg.name, img: expInstitute.bg.data })
      for(let i = 0; i < expInstitute.companies.length; i++) {
        imgList.push({ name: expInstitute.companies[i].name, img: expInstitute.companies[i].data })
      }
      for(let i = 0; i < expInstitute.reviews.length; i++) {
        imgList.push({ name: expInstitute.reviews[i].name, img: expInstitute.reviews[i].data })
      }
      for(let i = 0; i < expInstitute.programs.bachelor.length; i++) {
        imgList.push({ name: expInstitute.programs.bachelor[i].name, img: expInstitute.programs.bachelor[i].data })
      }
      for(let i = 0; i < expInstitute.programs.specialist.length; i++) {
        imgList.push({ name: expInstitute.programs.specialist[i].name, img: expInstitute.programs.specialist[i].data })
      }
      for(let i = 0; i < expInstitute.programs.master.length; i++) {
        imgList.push({ name: expInstitute.programs.master[i].name, img: expInstitute.programs.master[i].data })
      }
      const zip = new JSZip()
      zip.file(expInstitute.institute.home.shortname + ".json", JSON.stringify(expInstitute.institute, null, 2));
      imgList.forEach(item => {
        var url = new Promise((resolve, reject) => {
          JSZipUtils.getBinaryContent(item.img, function (err, data) {
            if(err) {
              reject(err);
            } else {
              resolve(data)
            }
          })
        })
        zip.file(item.name, url, {binary:true})
      })
      zip.generateAsync({type:"blob"})
      .then(function(content) {
        saveAs(content, expInstitute.institute.home.shortname + ".zip");
      });
    },
    importZip(event) {
      const file = event.target.files[0]
      var jszip = new JSZip();
      jszip.loadAsync(file).then((zip) => {
        this.resetAll() 
        this.resetLoaded() 
        for (let key in zip.files) { 
          if (!zip.files[key].dir) {
            if (/\.(json)$/.test(zip.files[key].name)) {
              let base = zip.file(zip.files[key].name).async('string')
              base.then(res => {
                this.institute = JSON.parse(res)
              })
            }
            if (/\.(png)$/.test(zip.files[key].name)) {  
              let base = zip.file(zip.files[key].name)
              let blob = new Blob([base._data.compressedContent], {type: "image/png"})
              if (/^company/.test(zip.files[key].name)) {
                this.companies.push({  loaded: true, name: zip.files[key].name, data: URL.createObjectURL(blob) });
              }
              if (/^review/.test(zip.files[key].name)) {
                this.reviews.push({ loaded: true, name: zip.files[key].name, data: URL.createObjectURL(blob) });
              }
              if (/^bg/.test(zip.files[key].name)) {
                this.bg = { loaded: true, name: zip.files[key].name, data: URL.createObjectURL(blob) };
              }
            }
            if (/\.(pdf)$/.test(zip.files[key].name)) {  
              let base = zip.file(zip.files[key].name)
              let blob = new Blob([base._data.compressedContent], {type: "application/pdf"})
              if (/^bac/.test(zip.files[key].name)) {
                this.programs.bachelor.push({ loaded: true, name: zip.files[key].name, data: URL.createObjectURL(blob)});
              }
              if (/^spec/.test(zip.files[key].name)) {
                this.programs.specialist.push({ loaded: true, name: zip.files[key].name, data: URL.createObjectURL(blob)});
              }
              if (/^mas/.test(zip.files[key].name)) {
                this.programs.master.push({ loaded: true, name: zip.files[key].name, data: URL.createObjectURL(blob)});
              }
            }
          }
        }
      })
    },
  }
}
</script>