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
              <img :src="institute.home.bg" onerror="this.src='https://www.aphroditebeach.gr/wp-content/uploads/2019/01/white.png'" 
                 class="object-cover rounded-lg h-24" />
            </div>
            <input type="file" ref="bg" class="hidden" @change="loadImg(institute.home.bg, 0,  $event)">
            <button v-show="!loadedB"
                    @click="openBg" 
                    class="btn-sm bg-gray-600 py-10 px-4 mr-2">
              <svg class="w-4 h-4 fill-current" viewBox="0 0 20 20"><path d="M10 3a1 1 0 011 1v5h5a1 1 0 110 2h-5v5a1 1 0 11-2 0v-5H4a1 1 0 110-2h5V4a1 1 0 011-1z" clip-rule="evenodd" fill-rule="evenodd"></path></svg>
            </button>
            <button v-show="loadedB"
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
                 type="text" 
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
              <button v-show="!loadedF[category][index].loaded" 
                      @click="openPdf(category, index)" 
                      class="btn-md bg-gray-600 py-4 px-32 ml-2">
                <svg class="w-4 h-4 fill-current" viewBox="0 0 20 20"><path d="M10 3a1 1 0 011 1v5h5a1 1 0 110 2h-5v5a1 1 0 11-2 0v-5H4a1 1 0 110-2h5V4a1 1 0 011-1z" clip-rule="evenodd" fill-rule="evenodd"></path></svg>
              </button>
              <button v-show="loadedF[category][index].loaded" 
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
            <img :src="company.logo" onerror="this.src='https://www.aphroditebeach.gr/wp-content/uploads/2019/01/white.png'" 
                 class="rounded-full mr-2 w-12 h-12 logo" />
            <input type="file" ref="image" class="hidden" @change="loadImg(institute.internship, index, $event)">
            <button v-show="!loadedI[index]" 
                    @click="openImg(index)" 
                    class="btn-sm bg-gray-600 py-4 px-4 mr-2">
              <svg class="w-4 h-4 fill-current" viewBox="0 0 20 20"><path d="M10 3a1 1 0 011 1v5h5a1 1 0 110 2h-5v5a1 1 0 11-2 0v-5H4a1 1 0 110-2h5V4a1 1 0 011-1z" clip-rule="evenodd" fill-rule="evenodd"></path></svg>
            </button>
            <button v-show="loadedI[index]" 
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
              <input v-model="reviews.name"  
                      type="text" 
                      placeholder="Name" 
                      class="field px-4 py-2 mr-2 w-2/3"/>
              <input v-model="reviews.role"  
                      type="text" 
                      placeholder="Role" 
                      class="field px-4 py-2 w-1/3"/>
            </div>
            <div class="flex w-full mt-4">
            <div class="rounded-lg mr-2 w-32 logo">
              <img :src="reviews.photo" onerror="this.src='https://www.aphroditebeach.gr/wp-content/uploads/2019/01/white.png'" 
                 class="object-cover rounded-lg h-40" />
            </div>
            <input type="file" ref="photo" class="hidden" @change="loadImg(institute.reviews, index, $event)">
            <button v-show="!loadedR[index]" 
                    @click="openPhoto(index)" 
                    class="btn-sm bg-gray-600 py-20 px-4 mr-2">
              <svg class="w-4 h-4 fill-current" viewBox="0 0 20 20"><path d="M10 3a1 1 0 011 1v5h5a1 1 0 110 2h-5v5a1 1 0 11-2 0v-5H4a1 1 0 110-2h5V4a1 1 0 011-1z" clip-rule="evenodd" fill-rule="evenodd"></path></svg>
            </button>
            <button v-show="loadedR[index]" 
                    @click="unloadImg(institute.reviews, index)"
                    class="btn-sm bg-red-500 py-20 px-4 mr-2">
              <svg class="icon w-4 h-4 fill-current" viewBox="0 0 20 20"><path d="M10 3a1 1 0 011 1v5h5a1 1 0 110 2h-5v5a1 1 0 11-2 0v-5H4a1 1 0 110-2h5V4a1 1 0 011-1z" clip-rule="evenodd" fill-rule="evenodd"></path></svg>
            </button>
            <textarea v-model="reviews.text"
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
      loadedI: [ false ],
      loadedR: [ false ],
      loadedB: false,
      loadedF: { bachelor: [{ loaded: false, data: "" }], specialist: [{ loaded: false, data: "" }], master: [{ loaded: false, data: "" }]},
      companies: [],
      reviews: [],
      defaultData: {}
    };
  },
  created() {
    this.defaultData = this.institute
  },
  methods: {
    addField(item) {
      if (item === this.institute.home.social) {
        item.push({ platform: "instagram"})
        return
      } 
      if (item === this.institute.internship) {
        this.loadedI.push(false);
        item.push({ logo: ""});
        return
      }
      if (item === this.institute.reviews) {
        this.loadedR.push(false);
        item.push({ photo: ""});
        return
      }
      if (this.institute.education.categories.includes(item)) {
        this.institute.education.programs[item].push({});
        this.loadedF[item].push({ loaded: false, data: "" });
        return
      }
      else {
        item.push({});
        return
      }
    },
    removeField(index, item) {
      if (item === this.institute.internship) {
        this.loadedI.pop();
        item.splice(index, 1);
      }
      if (item === this.institute.reviews) {
        this.loadedR.pop();
        item.splice(index, 1);
      }
      if (this.institute.education.categories.includes(item)) {
        this.institute.education.programs[item].splice(index, 1);
        this.loadedF[item].splice(index, 1);
      }
      else
        item.splice(index, 1);
    },
    loadImg(item, index, event) {
      const img = event.target.files[0]
      if (item === this.institute.internship) {
        this.institute.internship[index].logo = URL.createObjectURL(img);
        this.loadedI[index] = true
      }
      if (item === this.institute.reviews) {
        this.institute.reviews[index].photo = URL.createObjectURL(img);
        this.loadedR[index] = true
      }
      else {
        this.institute.home.bg = URL.createObjectURL(img);
        this.loadedB = true
      }
      this.$refs.img[index].value = ""
    },
    loadPdf(category, index, event) {
      const pd = event.target.files
      console.log(pd)
      const pdf = event.target.files[0]
      this.institute.education.programs[category][index].file = pdf.name
      this.loadedF[category][index].loaded = true
      this.loadedF[category][index].data = URL.createObjectURL(pdf)
      console.log(this.loadedF[category][index].data)
      this.$refs[category][index].value = ""
    },
    unloadImg(item, index) {
      if (item === this.institute.internship) {
        this.institute.internship[index].logo = ""
        this.loadedI[index] = false
      } 
      if (item === this.institute.reviews) {
        this.institute.reviews[index].photo = ""
        this.loadedR[index] = false
      } 
      else {
        this.institute.home.bg = ""
        this.loadedB = false
      }
    },
    unloadPdf(category, index) {
      this.institute.education.programs[category][index].file = ""
      this.loadedF[category][index].loaded = false
      this.loadedF[category][index].data = ""
    },
    loadJSON(event) {
      const file = event.target.files[0]
      const reader = new FileReader()
      reader.onload = (event) => {
        this.institute = JSON.parse(event.target.result);
      }
      reader.readAsText(file)
    },
    resetLoaded() {
      this.loadedI.splice(0,this.loadedI.length)
      this.loadedR.splice(0,this.loadedR.length)
      this.loadedB = false
    },
    resetImages() {
      this.companies.splice(0,this.companies.length)
      this.reviews.splice(0,this.reviews.length)
      this.institute = this.defaultData
    },
    resetAll() {
      this.resetLoaded()
      this.loadedI.push(false)
      this.loadedR.push(false)
      this.resetImages()
    },
    openImg(index){
      this.$refs.image[index].click()
    },
    openPhoto(index){
      this.$refs.photo[index].click()
    },
    openPdf(category, index) {
      if (category === "bachelor")
        this.$refs.bachelor[index].click()
      if (category === "specialist")
        this.$refs.specialist[index].click()
      if (category === "master")
        this.$refs.master[index].click()
    },
    openBg(){
      this.$refs.bg.click()
    },
    openFile(){
      this.$refs.file.click();
    },
    exportZip() {
      const imgList = []
      const expInstitute = JSON.parse(JSON.stringify(this.institute));
      for(let i = 0; i < expInstitute.internship.length; i++) {
        let filename = expInstitute.home.shortname + "-company" + i + ".png"
        imgList.push({name: filename, img: expInstitute.internship[i].logo })
        expInstitute.internship[i].logo = filename
      }
      for(let i = 0; i < expInstitute.reviews.length; i++) {
        let filename = expInstitute.home.shortname + "-review" + i + ".png"
        imgList.push({name: filename, img: expInstitute.reviews[i].photo })
        expInstitute.reviews[i].photo = filename
      }
      let filename = "bg-home.png"
      imgList.push({name: filename, img: expInstitute.home.bg })
      expInstitute.home.bg = filename
      const zip = new JSZip()
      zip.file(expInstitute.home.shortname + ".json", JSON.stringify(expInstitute, null, 2));
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
        saveAs(content, expInstitute.home.shortname + ".zip");
      });
    },
    importZip(event) {
      const file = event.target.files[0]
      var jszip = new JSZip();
      jszip.loadAsync(file).then((zip) => {
        this.resetLoaded() 
        for (let key in zip.files) { 
          if (!zip.files[key].dir) {
            if (/\.(json)$/.test(zip.files[key].name)) {
              let base = zip.file(zip.files[key].name).async('string')
              base.then(res => {
                var newInstitute = JSON.parse(res)
                for (var i = 0; i < this.companies.length; i++) {
                  newInstitute.internship[i].logo = this.companies[i].logo
                  this.loadedI.push(true)
                }
                for (var k = 0; k < this.reviews.length; k++) {
                  newInstitute.reviews[k].photo = this.reviews[k].photo
                  this.loadedR.push(true)
                }
                this.resetImages()
                this.institute = newInstitute
              })
            }
            if (/\.(png)$/.test(zip.files[key].name)) {  
              let base = zip.file(zip.files[key].name)
              let blob = new Blob([base._data.compressedContent], {type: "image/png"})
              if (/^company/.test(zip.files[key].name)) {
                this.companies.push({ logo: URL.createObjectURL(blob)});
              }
              if (/^review/.test(zip.files[key].name)) {
                this.reviews.push({ photo: URL.createObjectURL(blob)});
              }
            }
            if (/\.(pdf)$/.test(zip.files[key].name)) {  
              let base = zip.file(zip.files[key].name)
              let blob = new Blob([base._data.compressedContent], {type: "application/pdf"})
              if (/^bac/.test(zip.files[key].name)) {
                this.loadedF.bachelor.push({ loaded: true, data: URL.createObjectURL(blob)});
              }
              if (/^spec/.test(zip.files[key].name)) {
                this.loadedF.specialist.push({ loaded: true, data: URL.createObjectURL(blob)});
              }
              if (/^mas/.test(zip.files[key].name)) {
                this.loadedF.master.push({ loaded: true, data: URL.createObjectURL(blob)});
              }
            }
          }
        }
      })
    },
  }
}
</script>