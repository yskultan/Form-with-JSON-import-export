<template>
  <div class="bg-white w-full sm:px-4 md:px-16 lg:px-48 sm:py-2 md:py-4 lg:py-8">
    <div class="h-full mx-auto p-16">
      <div class="part p-6 w-full mb-8">
        <span class="title mb-8">Main</span>
        <label class="subtitle mb-2">
              Institute name
        </label>
        <input id="name" 
              v-model="institute.home.name"
              type="text"
              class="field px-4 py-2 w-full" 
              placeholder="Institute of Space and Information Technologies"/>
        <label class="subtitle mt-8 mb-2">
              Social links
        </label>
        <div v-for="(input, index) in institute.home.social"
            :key="`social-${index}`"
            class="flex mt-2"
          >
          <select v-model="input.platform" class="field w-1/3 py-2 px-4">
              <option value="instagram" selected>Instagram</option>
              <option value="youtube">Youtube</option>
              <option value="vk">VK</option>
              <option value="facebook">Facebook</option>
          </select>
          <input v-model="input.link" 
                 type="text" 
                 placeholder="Link" 
                 class="field ml-2 px-4 py-2 w-2/3" />
        </div>
        <button @click="addField(institute.home.social)" 
                class="btn-sm bg-gray-600 py-1 px-3 mt-3">
                Another link
        </button>
        <button v-show="institute.home.social.length > 1" 
                @click="removeField(institute.home.social.length--, institute.home.social)" 
                class="btn-sm bg-red-400 py-1 px-3 ml-2 mt-3">
                Delete link
        </button>
      </div>
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
      <div class="part p-6 w-full mb-8">
        <span class="title mb-8">Our numbers</span>
        <label class="subtitle mb-2">
          Important numbers
        </label>
        <div v-for="(input, index) in institute.numbers" :key="`numbers-${index}`" class="flex mt-2">
            <input v-model="input.number"
                   class="field px-4 py-2 w-1/2" 
                   type="number" 
                   placeholder="Number" />
            <input v-model="input.text"
                   class="field px-4 py-2 ml-4 w-1/2" 
                   type="text" 
                   placeholder="Text" />
        </div>
        <button v-show="institute.numbers.length < 6" 
                @click="addField(institute.numbers)" 
                class="btn-sm bg-gray-600 py-1 px-3 mr-2 mt-3">
                Another number
        </button>
        <button v-show="institute.numbers.length > 1" 
                @click="removeField(institute.numbers.length--, institute.numbers)" 
                class="btn-sm bg-red-400 py-1 px-3 mt-3">
                Delete number
        </button>
      </div>
      <div class="flex mb-8">
        <div class="part p-6 w-full">
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
              <div class="flex w-full">
                <input v-model="item.title"  
                        type="text" 
                        placeholder="Title" 
                        class="field px-4 py-2 mr-2 w-2/3"/>
                <input v-model="item.code"  
                        type="text" 
                        placeholder="Code" 
                        class="field px-4 py-2 w-1/3"/>
              </div>
              <textarea v-model="item.description"
                        class="field px-4 py-2 mt-2 w-full h-40" 
                        placeholder="Description">
              </textarea>
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
            </div>
            <button @click="addField(institute.education.programs[category])" 
                    class="btn-sm bg-gray-600 py-1 px-3 mt-3">
              Another program
            </button>
            <button v-show="institute.education.programs[category].length > 1" 
                    @click="removeField(institute.education.programs[category].length--, institute.education.programs[category])" 
                    class="btn-sm bg-red-400 py-1 px-3 ml-2 mt-3">
              Delete program
            </button>
          </div>
        </div>
      </div>
      <div class="flex w-full">
        <button @click="generateJSON" 
                class="bg-orange-600 btn-md py-3 px-6 mt-3">
            Save
        </button>
        <button class="bg-gray-600 ml-4 btn-md py-3 px-6 mt-3" 
                @click="openFile">
            Load file
        </button>
        <input type="file" ref="file" accept="application/json" class="hidden" @change="loadJSON">
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'SibForm',
  data() {
    return {
      institute: {
        home: {
          name: "",
          social: [{ platform:"instagram", link: "" }],
        },
        about: "",
        numbers: [{ number: "", text: "" }],
        education: {
          categories: ["bachelor"],
          programs: {
            bachelor: [{ title: "", code: "", description: "", years: "", tuition: "" }],
            specialist: [{ title: "", code: "", description: "", years: "", tuition: "" }],
            master: [{ title: "", code: "", description: "", years: "", tuition: "" }]
          }
        }
      }
    };
  },
  methods: {
    addField(fieldType) {
      if (fieldType === this.institute.home.social) {
        fieldType.push({ platform: "instagram"});
      } else
        fieldType.push({});
    },
    removeField(index, fieldType) {
      fieldType.splice(index, 1);
    },
    generateJSON() {
      const results = JSON.stringify(this.institute, null, 2);
      var blob = new Blob([results], {type:'application/json'});
      var link = document.createElement("a");
      link.download = "data.json";
      link.href = window.URL.createObjectURL(blob);
      link.click();
    },
    loadJSON(event) {
      const file = event.target.files[0]
      const fr = new FileReader()
      fr.onload = (event) => {
        this.institute = JSON.parse(event.target.result);
      }
      fr.readAsText(file)
    },
    openFile(){
      this.$refs.file.click();
      console.log("opened")
    }
  }
}
</script>

<style scoped>
input:checked + svg {
  display: block
}
</style>
