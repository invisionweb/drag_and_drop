<template>
  <div class="flex flex-col lg:flex-row p-6 gap-4 lg:justify-between">
    <div class="flex space-x-2 justify-center items-center">
      <h1 class="lg:text-2xl font-medium text-gray-500">UI Builder for</h1>
      <a href="https://tailwindcss.com" target="_blank">
        <img
            alt="TailwindCSS"
            class="h-6 w-auto"
            src="https://tailwindcss.com/_next/static/media/tailwindcss-logotype.128b6e12eb85d013bc9f80a917f57efe.svg"
        />
      </a>
    </div>
    <div class="flex flex-row space-x-2 justify-center">
      <button class="inline-flex text-sm items-center border p-2 rounded-md font-medium hover:bg-gray-200"
              title="Clear Canvas" @click="clear_canvas">
        <svg class="h-5 w-5 text-red-500 mr-1" fill="none" stroke="currentColor" viewBox="0 0 24 24"
             xmlns="http://www.w3.org/2000/svg">
          <path
              d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"
              stroke-linecap="round" stroke-linejoin="round"
              stroke-width="2"/>
        </svg>
        Clear canvas
      </button>
<!--      <button class="inline-flex text-sm items-center border p-2 rounded-md font-medium hover:bg-gray-200"
              title="Preview" @click="clear_canvas">
        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
             class="h-5 w-5 text-gray-500 mr-2" viewBox="0 0 16 16">
          <path
              d="M0 4s0-2 2-2h12s2 0 2 2v6s0 2-2 2h-4c0 .667.083 1.167.25 1.5H11a.5.5 0 0 1 0 1H5a.5.5 0 0 1 0-1h.75c.167-.333.25-.833.25-1.5H2s-2 0-2-2V4zm1.398-.855a.758.758 0 0 0-.254.302A1.46 1.46 0 0 0 1 4.01V10c0 .325.078.502.145.602.07.105.17.188.302.254a1.464 1.464 0 0 0 .538.143L2.01 11H14c.325 0 .502-.078.602-.145a.758.758 0 0 0 .254-.302 1.464 1.464 0 0 0 .143-.538L15 9.99V4c0-.325-.078-.502-.145-.602a.757.757 0 0 0-.302-.254A1.46 1.46 0 0 0 13.99 3H2c-.325 0-.502.078-.602.145z"/>
        </svg>
        Preview
      </button>-->
      <button
          class="inline-flex text-sm items-center p-2 rounded-md font-medium bg-indigo-600 hover:bg-indigo-400 text-white"
          title="Get code" @click="get_code">
        <svg class="h-5 w-5 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24"
             xmlns="http://www.w3.org/2000/svg">
          <path d="M10 20l4-16m4 4l4 4-4 4M6 16l-4-4 4-4" stroke-linecap="round" stroke-linejoin="round"
                stroke-width="2"/>
        </svg>
        Create code
      </button>
      <!-- <button @click="clear_canvas">Preview</button>-->
    </div>
  </div>
  <div class="m-4" :class="{ hidden: !show_code_mirror}" id="code-mirror-editor"></div>
  <div class="grid grid-cols-8">
    <div class="flex flex-col col-span-8 lg:col-span-1 p-2 bg-gray-50" title="Drag Elements to canvas">
      <!-- <div class="draggable cursor-move select-none">Container</div> -->
      <!-- <div class="draggable cursor-move select-none" id="grid">Grid</div> -->
      <div id="flex" class="p-2 draggable select-none" @click="add_element">
        <div class="w-full p-2 inline-flex items-center space-x-2 bg-indigo-100 bg-stripes bg-stripes-white rounded-md">
          <div class="rounded-md text-white font-extrabold text-center border-2 border-dashed border-black h-6 w-6"></div>
          <span class="text-xs text-gray-500">Container</span>
          <!-- <div class="rounded-md text-white font-extrabold text-center bg-indigo-400 h-6 w-6"></div>
          <div class="rounded-md text-white font-extrabold text-center bg-indigo-400 h-6 w-6"></div>-->
        </div>
      </div>
      <div id="text" @click="add_element" class="p-2 draggable select-none inline-flex items-center text-sm text-gray-500">
        <svg class="bi bi-textarea-t" fill="currentColor" height="16" viewBox="0 0 16 16" width="16"
             xmlns="http://www.w3.org/2000/svg">
          <path
              d="M1.5 2.5A1.5 1.5 0 0 1 3 1h10a1.5 1.5 0 0 1 1.5 1.5v3.563a2 2 0 0 1 0 3.874V13.5A1.5 1.5 0 0 1 13 15H3a1.5 1.5 0 0 1-1.5-1.5V9.937a2 2 0 0 1 0-3.874V2.5zm1 3.563a2 2 0 0 1 0 3.874V13.5a.5.5 0 0 0 .5.5h10a.5.5 0 0 0 .5-.5V9.937a2 2 0 0 1 0-3.874V2.5A.5.5 0 0 0 13 2H3a.5.5 0 0 0-.5.5v3.563zM2 7a1 1 0 1 0 0 2 1 1 0 0 0 0-2zm12 0a1 1 0 1 0 0 2 1 1 0 0 0 0-2z"/>
          <path
              d="M11.434 4H4.566L4.5 5.994h.386c.21-1.252.612-1.446 2.173-1.495l.343-.011v6.343c0 .537-.116.665-1.049.748V12h3.294v-.421c-.938-.083-1.054-.21-1.054-.748V4.488l.348.01c1.56.05 1.963.244 2.173 1.496h.386L11.434 4z"/>
        </svg>
        <span class="ml-2">TEXT</span>
      </div>
      <div id="button" @click="add_element" class="p-2 draggable select-none inline-flex items-center text-sm text-gray-500">
        <button class="bg-black text-white text-xs p-2 rounded-md">Button</button>
      </div>
      <div id="input" @click="add_element" class="p-2 draggable select-none">
        <div class="w-full text-xs border border-indigo-600 outline-none rounded p-2">
          Input field
        </div>
      </div>
      <div id="img" @click="add_element" class="p-1 draggable select-none text-indigo-700">
        <svg class="h-10 w-10" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
          <path
              d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z"
              stroke-linecap="round" stroke-linejoin="round"
              stroke-width="2"/>
        </svg>
      </div>
      <div id="table" @click="add_element" class="p-2 draggable cursor-move select-none hidden">
        <div class="w-full p-2 inline-flex items-center space-x-2 bg-indigo-100 bg-stripes bg-stripes-white rounded-md">
          <div class="rounded-md text-indigo-700 font-extrabold text-center h-6 w-6">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24"
                 stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                    d="M3 10h18M3 14h18m-9-4v8m-7 0h14a2 2 0 002-2V8a2 2 0 00-2-2H5a2 2 0 00-2 2v8a2 2 0 002 2z"/>
            </svg>
          </div>
          <span class="text-xs text-gray-500">Table</span>
          <!-- <div class="rounded-md text-white font-extrabold text-center bg-indigo-400 h-6 w-6"></div>
          <div class="rounded-md text-white font-extrabold text-center bg-indigo-400 h-6 w-6"></div>-->
        </div>
      </div>
      <div id="list" class="p-2 draggable cursor-move select-none hidden">
        <div class="w-full p-2 inline-flex items-center space-x-2 bg-indigo-100 bg-stripes bg-stripes-white rounded-md">
          <div class="rounded-md text-indigo-700 font-extrabold text-center h-6 w-6">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24"
                 stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                    d="M4 6h16M4 10h16M4 14h16M4 18h16"/>
            </svg>
          </div>
          <span class="text-xs text-gray-500">List</span>
          <!-- <div class="rounded-md text-white font-extrabold text-center bg-indigo-400 h-6 w-6"></div>
          <div class="rounded-md text-white font-extrabold text-center bg-indigo-400 h-6 w-6"></div>-->
        </div>
      </div>
      <div id="add-element" class="p-2 draggable cursor-move select-none hidden">
        <div class="w-full p-2 inline-flex items-center space-x-2 bg-indigo-100 bg-stripes bg-stripes-white rounded-md">
          <div class="rounded-md text-indigo-700 font-extrabold text-center h-6 w-6">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24"
                 stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                    d="M17 14v6m-3-3h6M6 10h2a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v2a2 2 0 002 2zm10 0h2a2 2 0 002-2V6a2 2 0 00-2-2h-2a2 2 0 00-2 2v2a2 2 0 002 2zM6 20h2a2 2 0 002-2v-2a2 2 0 00-2-2H6a2 2 0 00-2 2v2a2 2 0 002 2z"/>
            </svg>
          </div>
          <span class="text-xs text-gray-500">Element</span>
          <!-- <div class="rounded-md text-white font-extrabold text-center bg-indigo-400 h-6 w-6"></div>
          <div class="rounded-md text-white font-extrabold text-center bg-indigo-400 h-6 w-6"></div>-->
        </div>
      </div>
    </div>
    <div class="col-span-8 lg:col-span-5 lg:pl-4">

      <div class="sticky top-4 p-4 lg:p-0">

      <div class="bg-yellow-50 text-sm text-gray-500 py-2 mb-4 px-4 inline-flex items-center gap-2 rounded-md w-full">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 15l-2 5L9 9l11 4-5 2zm0 0l5 5M7.188 2.239l.777 2.897M5.136 7.965l-2.898-.777M13.95 4.05l-2.122 2.122m-5.657 5.656l-2.12 2.122" />
        </svg>
        <span>Drag and drop HTML elements in the canvas using cursor.</span>
      </div>

        <div class="overflow-hidden bg-white border border-gray-300 rounded-md text-gray-500 text-xs">
          <h3 class="py-1 px-2 bg-gray-100"><span id="element" class="hidden"></span> Tailwind Classes</h3>
          <div class="flex flex-wrap gap-2 p-2">
            <input id="add-class" type="text" v-on:keyup.enter="add_class" class="outline-none" placeholder="Write class name">
            <template v-for="(class_name,index) in selected_element_classes.split(/\s+/)" v-bind:key="index">
              <button v-if="class_name.length > 0 && !/selected-element|ui|droppable-flex-container/.test(class_name)" v-on:click="remove_class(class_name)" class="flex rounded-full bg-indigo-500 hover:bg-red-500 text-white pl-2 items-center">
                .{{ class_name }}
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 ml-1" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zM8.707 7.293a1 1 0 00-1.414 1.414L8.586 10l-1.293 1.293a1 1 0 101.414 1.414L10 11.414l1.293 1.293a1 1 0 001.414-1.414L11.414 10l1.293-1.293a1 1 0 00-1.414-1.414L10 8.586 8.707 7.293z" clip-rule="evenodd" />
                </svg>
              </button>
            </template>
          </div>
        </div>

        <div id="draggable-items-container" class="border border-dashed overflow-y-auto h-96 my-4"></div>
        <div id="main-canvas" class="droppable min-h-full w-full border hidden"></div>
      </div>

    </div>

    <div class="px-4 pb-4 flex flex-col space-y-4 col-span-8 lg:col-span-2">
      <div class="border border-gray-300 divide-x divide-gray-500 flex items-center space-x-2 p-2 rounded-md">
        <button @click="duplicate_element">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                  d="M8 16H6a2 2 0 01-2-2V6a2 2 0 012-2h8a2 2 0 012 2v2m-6 12h8a2 2 0 002-2v-8a2 2 0 00-2-2h-8a2 2 0 00-2 2v8a2 2 0 002 2z"/>
          </svg>
        </button>
        <button class="pl-2" @click="delete_element">
          <svg class="h-5 w-5 text-red-500" fill="none" stroke="currentColor" viewBox="0 0 24 24"
               xmlns="http://www.w3.org/2000/svg">
            <path
                d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"
                stroke-linecap="round" stroke-linejoin="round"
                stroke-width="2"/>
          </svg>
        </button>
        <button class="pl-2 hidden" @click="remove_frame_border">
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-x-square"
               viewBox="0 0 16 16">
            <path
                d="M14 1a1 1 0 0 1 1 1v12a1 1 0 0 1-1 1H2a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1h12zM2 0a2 2 0 0 0-2 2v12a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V2a2 2 0 0 0-2-2H2z"/>
            <path
                d="M4.646 4.646a.5.5 0 0 1 .708 0L8 7.293l2.646-2.647a.5.5 0 0 1 .708.708L8.707 8l2.647 2.646a.5.5 0 0 1-.708.708L8 8.707l-2.646 2.647a.5.5 0 0 1-.708-.708L7.293 8 4.646 5.354a.5.5 0 0 1 0-.708z"/>
          </svg>
        </button>
      </div>
      <!--      <textarea id="selected_element_classes"
                  class="border border-gray-300 outline-none p-2 rounded-md text-gray-500 text-sm"
                  placeholder="Classes"></textarea>-->
      <div id="selected_element_classes" class="hidden"></div>
      <textarea v-model="selected_element_inner_html"
             class="w-full border border-gray-300 outline-none p-2 rounded-md text-gray-500 text-sm"
                placeholder="HTML"></textarea>
      <textarea id="element-html" v-model="selected_element_html" class="border border-black hidden"
                @keyup="element_html_change"></textarea>
      <!-- <div>
        Attributes
      </div>-->
      <div id="flex-style" class="border rounded text-sm text-gray-600 flex flex-col divide-y">
        <h3 class="py-1 px-2 bg-gray-100">Flex</h3>

        <div>
          <button data-template="flex-direction" class="w-full p-2 hover:bg-indigo-50 properties">Flex direction</button>
        </div>
        <div class="hidden">
          <div id="flex-direction">
            <div class="flex flex-col divide-y space-y-2 my-1">
              <div data-class="flex-row" class="flex flex-row bg-indigo-50 w-72 border rounded p-1">
                <div
                    data-class="flex-row" class="w-8 h-8 text-white text-xl font-extrabold rounded-md flex items-center justify-center bg-indigo-300 m-1">
                  1
                </div>
                <div
                    data-class="flex-row" class="w-8 h-8 text-white text-xl font-extrabold rounded-md flex items-center justify-center bg-indigo-300 m-1">
                  2
                </div>
                <div
                    data-class="flex-row" class="w-8 h-8 text-white text-xl font-extrabold rounded-md flex items-center justify-center bg-indigo-300 m-1">
                  3
                </div>
              </div>
              <div data-class="flex-row-reverse" class="flex flex-row-reverse bg-indigo-50 w-72 border rounded p-1">
                <div
                    data-class="flex-row-reverse" class="w-8 h-8 text-white text-xl font-extrabold rounded-md flex items-center justify-center bg-indigo-300 m-1">
                  1
                </div>
                <div
                    data-class="flex-row-reverse" class="w-8 h-8 text-white text-xl font-extrabold rounded-md flex items-center justify-center bg-indigo-300 m-1">
                  2
                </div>
                <div
                    data-class="flex-row-reverse" class="w-8 h-8 text-white text-xl font-extrabold rounded-md flex items-center justify-center bg-indigo-300 m-1">
                  3
                </div>
              </div>
              <div data-class="flex-col" class="flex flex-col bg-indigo-50 w-72 border rounded p-1">
                <div
                    data-class="flex-col" class="w-8 h-8 text-white text-xl font-extrabold rounded-md flex items-center justify-center bg-indigo-300 m-1">
                  1
                </div>
                <div
                    data-class="flex-col" class="w-8 h-8 text-white text-xl font-extrabold rounded-md flex items-center justify-center bg-indigo-300 m-1">
                  2
                </div>
                <div
                    data-class="flex-col" class="w-8 h-8 text-white text-xl font-extrabold rounded-md flex items-center justify-center bg-indigo-300 m-1">
                  3
                </div>
              </div>
              <div data-class="flex-col-reverse" class="flex flex-col-reverse bg-indigo-50 w-72 border rounded p-1">
                <div
                    data-class="flex-col-reverse" class="w-8 h-8 text-white text-xl font-extrabold rounded-md flex items-center justify-center bg-indigo-300 m-1">
                  1
                </div>
                <div
                    data-class="flex-col-reverse" class="w-8 h-8 text-white text-xl font-extrabold rounded-md flex items-center justify-center bg-indigo-300 m-1">
                  2
                </div>
                <div
                    data-class="flex-col-reverse" class="w-8 h-8 text-white text-xl font-extrabold rounded-md flex items-center justify-center bg-indigo-300 m-1">
                  3
                </div>
              </div>
            </div>
          </div>
        </div>

        <button data-template="justify-content" class="p-2 hover:bg-indigo-100">Justify Content</button>
        <!--        <select id="flex-row-or-col" @change="add_class">
                  <option>
                    flex-row
                  </option>
                  <option>flex-col</option>
                </select>-->
      </div>

      <div class="border rounded text-sm text-gray-600 flex flex-col divide-y">
        <h3 class="py-1 px-2 bg-gray-100">Margin</h3>
        <div class="flex col-span-4 px-2 pb-2 pt-1 gap-1">
          <div class="flex flex-col w-1/4">
            <label for="mt" class="text-xs font-semibold text-gray-500 my-1">Top</label>
            <select id="mt" @change="add_class"
                    class="appearance-none bg-white text-xs border border-gray-300 px-2 py-1 w-full">
              <template v-for="margin in margins" :key="margin">
                <option v-if="/mt/gi.test(margin)">{{ margin }}</option>
              </template>
            </select>
          </div>
          <div class="flex flex-col w-1/4">
            <label for="mr" class="text-xs font-semibold text-gray-500 my-1">Right</label>
            <select id="mr" @change="add_class" class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <option v-for="margin in margins" :key="margin">{{ margin }}</option>
            </select>
          </div>
          <div class="flex flex-col w-1/4">
            <label for="mb" class="text-xs font-semibold text-gray-500 my-1">Bottom</label>
            <select id="mb" @change="add_class" class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <option v-for="margin in margins" :key="margin">{{ margin }}</option>
            </select>
          </div>
          <div class="flex flex-col w-1/4">
            <label for="ml" class="text-xs font-semibold text-gray-500 my-1">Left</label>
            <select id="ml" @change="add_class" class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <option v-for="margin in margins" :key="margin">{{ margin }}</option>
            </select>
          </div>
        </div>
      </div>

      <div class="border rounded text-sm text-gray-600 flex flex-col divide-y">
        <h3 class="py-1 px-2 bg-gray-100">Padding</h3>
        <div class="flex col-span-4 px-2 pb-2 pt-1 gap-1">
          <div class="flex flex-col w-1/4">
            <label for="pt" class="text-xs font-semibold text-gray-500 my-1">Top</label>
            <select id="pt" @change="add_class" class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <option v-for="pading in paddings" :key="pading">{{ pading }}</option>
            </select>
          </div>
          <div class="flex flex-col w-1/4">
            <label for="pr" class="text-xs font-semibold text-gray-500 my-1">Right</label>
            <select id="pr" @change="add_class" class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <option v-for="pading in paddings" :key="pading">{{ pading }}</option>
            </select>
          </div>
          <div class="flex flex-col w-1/4">
            <label for="pb" class="text-xs font-semibold text-gray-500 my-1">Bottom</label>
            <select id="pb" @change="add_class" class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <option v-for="pading in paddings" :key="pading">{{ pading }}</option>
            </select>
          </div>
          <div class="flex flex-col w-1/4">
            <label for="pl" class="text-xs font-semibold text-gray-500 my-1">Left</label>
            <select id="pl" @change="add_class" class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <option v-for="pading in paddings" :key="pading">{{ pading }}</option>
            </select>
          </div>
        </div>
      </div>

      <div class="border rounded text-sm text-gray-600 flex flex-col divide-y">
        <h3 class="py-1 px-2 bg-gray-100">Text style</h3>
        <div class="flex col-span-4 px-2 pb-2 pt-1 gap-1">
          <div class="flex flex-col w-1/4">
            <label for="text-size" class="text-xs font-semibold text-gray-500 my-1">Size</label>
            <select id="text-size" @change="add_class"
                    class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <option v-for="class_name in text_sizes" :key="class_name">{{ class_name }}</option>
            </select>
          </div>
          <div class="flex flex-col w-1/4">
            <label for="text-color" class="text-xs font-semibold text-gray-500 my-1">Color</label>
            <select id="text-color" @change="add_class"
                    class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <option v-for="class_name in text_colors" :key="class_name">{{ class_name }}</option>
            </select>
          </div>
          <div class="flex flex-col w-1/4">
            <label for="text-font" class="text-xs font-semibold text-gray-500 my-1">Font</label>
            <select id="text-font" @change="add_class"
                    class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <option v-for="class_name in font_families" :key="class_name">{{ class_name }}</option>
            </select>
          </div>
          <div class="flex flex-col w-1/4">
            <label for="text-weight" class="text-xs font-semibold text-gray-500 my-1">Weight</label>
            <select id="text-weight" @change="add_class"
                    class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <option v-for="class_name in font_weights" :key="class_name">{{ class_name }}</option>
            </select>
          </div>
        </div>
      </div>

      <div class="border rounded text-sm text-gray-600 flex flex-col divide-y">
        <h3 class="py-1 px-2 bg-gray-100">Border</h3>
        <div class="flex col-span-3 px-2 pb-2 pt-1 gap-1">
          <div class="flex flex-col w-1/3">
            <label for="border-color" class="text-xs font-semibold text-gray-500 my-1">Color</label>
            <select id="border-color" @change="add_class"
                    class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <option v-for="border_color in border_colors" :key="border_color">{{ border_color }}</option>
            </select>
          </div>
          <div class="flex flex-col w-1/3">
            <label for="border-width" class="text-xs font-semibold text-gray-500 my-1">Width</label>
            <select id="border-width" @change="add_class"
                    class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <option v-for="border_width in border_widths" :key="border_width">{{ border_width }}</option>
            </select>
          </div>
          <div class="flex flex-col w-1/3">
            <label for="border-style" class="text-xs font-semibold text-gray-500 my-1">Style</label>
            <select id="border-style" @change="add_class"
                    class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <option v-for="border_style in border_styles" :key="border_style">{{ border_style }}</option>
            </select>
          </div>
        </div>
      </div>

      <div class="border rounded text-sm text-gray-600 flex flex-col divide-y">
        <h3 class="py-1 px-2 bg-gray-100">Width & Height</h3>
        <div class="flex col-span-4 px-2 pb-2 pt-1 gap-1">
          <div class="flex flex-col w-1/2">
            <label for="w" class="text-xs font-semibold text-gray-500 my-1">Width</label>
            <select id="w" @change="add_class" class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <option v-for="width in widths" :key="width">{{ width }}</option>
            </select>
          </div>
          <div class="flex flex-col w-1/2">
            <label for="h" class="text-xs font-semibold text-gray-500 my-1">Height</label>
            <select id="h" @change="add_class" class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <option v-for="height in heights" :key="height">{{ height }}</option>
            </select>
          </div>
        </div>
      </div>

      <div class="border rounded text-sm text-gray-600 flex flex-col divide-y hidden">
        <h3 class="py-1 px-2 bg-gray-100">Ring</h3>
        <div class="flex col-span-4 px-2 pb-2 pt-1 gap-1">
          <div class="flex flex-col w-1/4">
            <label class="text-xs font-semibold text-gray-500 my-1">Top</label>
            <select id="p" @change="add_class" class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <option v-for="pading in paddings" :key="pading">{{ pading }}</option>
            </select>
          </div>
          <div class="flex flex-col w-1/4">
            <label class="text-xs font-semibold text-gray-500 my-1">Right</label>
            <select id="pr" @change="add_class" class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <option v-for="pading in paddings" :key="pading">{{ pading }}</option>
            </select>
          </div>
          <div class="flex flex-col w-1/4">
            <label class="text-xs font-semibold text-gray-500 my-1">Bottom</label>
            <select id="pb" @change="add_class" class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <option v-for="pading in paddings" :key="pading">{{ pading }}</option>
            </select>
          </div>
          <div class="flex flex-col w-1/4">
            <label class="text-xs font-semibold text-gray-500 my-1">Left</label>
            <select id="pl" @change="add_class" class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <option v-for="pading in paddings" :key="pading">{{ pading }}</option>
            </select>
          </div>
        </div>
        <div class="flex col-span-4 px-2 pb-2 pt-1 gap-1">
          <div class="flex flex-col w-1/2">
            <label class="text-xs font-semibold text-gray-500 my-1">Color</label>
            <select id="ring-color" @change="add_class"
                    class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <option v-for="ring_color in ring_colors" :key="ring_color">{{ ring_color }}</option>
            </select>
          </div>
          <div class="flex flex-col w-1/2">
            <label class="text-xs font-semibold text-gray-500 my-1">Style</label>
            <select id="pr" @change="add_class" class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <option v-for="pading in paddings" :key="pading">{{ pading }}</option>
            </select>
          </div>
        </div>
      </div>

      <div class="border rounded text-sm text-gray-600 flex flex-col divide-y">
        <h3 class="py-1 px-2 bg-gray-100">Position</h3>
        <div class="flex col-span-4 px-2 pb-2 pt-1 gap-1">
          <div class="flex flex-col w-1/2">
            <label for="position-type" class="text-xs font-semibold text-gray-500 my-1">Type</label>
            <select id="position-type" @change="add_class"
                    class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <option v-for="position in positions" :key="position">{{ position }}</option>
            </select>
          </div>
          <div class="flex flex-col w-1/2">
            <label for="position-inset" class="text-xs font-semibold text-gray-500 my-1">Inset</label>
            <select data-regex="inset" id="position-inset" @change="add_class" class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <template v-for="trbli in trblis" :key="trbli">
                <option v-if="/inset/gi.test(trbli)">{{ trbli }}</option>
              </template>
            </select>
          </div>
        </div>
        <div class="flex col-span-4 px-2 pb-2 pt-1 gap-1">
          <div class="flex flex-col w-1/4">
            <label for="position-top" class="text-xs font-semibold text-gray-500 my-1">Top</label>
            <select data-regex="top" id="position-top" @change="add_class" class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <template v-for="trbli in trblis" :key="trbli">
                <option v-if="/top/gi.test(trbli)">{{ trbli }}</option>
              </template>
            </select>
          </div>
          <div class="flex flex-col w-1/4">
            <label for="position-right" class="text-xs font-semibold text-gray-500 my-1">Right</label>
            <select data-regex="right" id="position-right" @change="add_class" class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <template v-for="trbli in trblis" :key="trbli">
                <option v-if="/right/gi.test(trbli)">{{ trbli }}</option>
              </template>
            </select>
          </div>
          <div class="flex flex-col w-1/4">
            <label for="position-bottom" class="text-xs font-semibold text-gray-500 my-1">Bottom</label>
            <select data-regex="bottom" id="position-bottom" @change="add_class" class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <template v-for="trbli in trblis" :key="trbli">
                <option v-if="/bottom/gi.test(trbli)">{{ trbli }}</option>
              </template>
            </select>
          </div>
          <div class="flex flex-col w-1/4">
            <label for="position-left" class="text-xs font-semibold text-gray-500 my-1">Left</label>
            <select data-regex="left" id="position-left" @change="add_class" class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <template v-for="trbli in trblis" :key="trbli">
                <option v-if="/left/gi.test(trbli)">{{ trbli }}</option>
              </template>
            </select>
          </div>
        </div>
      </div>

      <div class="border rounded text-sm text-gray-600 flex flex-col divide-y">
        <h3 class="py-1 px-2 bg-gray-100">Background</h3>
        <div class="flex col-span-1 px-2 pb-2 pt-1 gap-1">
          <div class="flex flex-col">
            <label for="bg" class="text-xs font-semibold text-gray-500 my-1">Color</label>
            <select id="bg" @change="add_class"
                    class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
              <option v-for="bg_color in bg_colors" :key="bg_color">{{ bg_color }}</option>
            </select>
          </div>
        </div>
      </div>

      <div class="flex flex-col">
        <label for="shadow" class="text-xs font-semibold text-gray-500 my-1">Shadow</label>
        <select id="shadow" @change="add_class"
                class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
          <option v-for="shadow in shadows" :key="shadow">{{ shadow }}</option>
        </select>
      </div>
      <div class="flex flex-col">
        <label for="rounded" class="text-xs font-semibold text-gray-500 my-1">Radius</label>
        <select id="rounded" @change="add_class"
                class="appearance-none text-xs border border-gray-300 px-2 py-1 w-full">
          <option v-for="border_radius in border_radiuses" :key="border_radius">{{ border_radius }}</option>
        </select>
      </div>
    </div>
  </div>
  <pre
      id="code-editor"
      class="hidden overflow-x-auto mx-auto my-8 outline-none max-w-4xl border resize font-mono text-sm p-2"
      contenteditable
  ></pre>
  <div id="code_editor" class="border rounded-md m-6 hidden"></div>
  <div id="template" class="hidden">
    <strong>Bolded content</strong>
  </div>

  <TransitionRoot :show="isOpen" appear as="template">
    <Dialog as="div" @close="closeModal">
      <div class="fixed inset-0 z-10 overflow-y-auto">
        <div class="min-h-screen px-4 text-center">
          <TransitionChild
              as="template"
              enter="duration-300 ease-out"
              enter-from="opacity-0"
              enter-to="opacity-100"
              leave="duration-200 ease-in"
              leave-from="opacity-100"
              leave-to="opacity-0"
          >
            <DialogOverlay class="fixed inset-0"/>
          </TransitionChild>

          <span aria-hidden="true" class="inline-block h-screen align-middle">
            &#8203;
          </span>

          <TransitionChild
              as="template"
              enter="duration-300 ease-out"
              enter-from="opacity-0 scale-95"
              enter-to="opacity-100 scale-100"
              leave="duration-200 ease-in"
              leave-from="opacity-100 scale-100"
              leave-to="opacity-0 scale-95"
          >
            <div
                class="inline-block w-full max-w-md p-6 my-8 overflow-hidden text-left align-middle transition-all transform bg-white shadow-xl rounded-2xl"
            >
              <!--              <DialogTitle
                                as="h3"
                                class="text-lg font-medium leading-6 text-gray-900"
                            >
                              Payment successful
                            </DialogTitle>-->
              <div class="mt-2">
                <p class="text-sm text-gray-500">
                  {{ modal_message }}
                </p>
              </div>

              <div class="mt-4">
                <button
                    class="inline-flex justify-center px-4 py-2 text-sm font-medium text-blue-900 bg-blue-100 border border-transparent rounded-md hover:bg-blue-200 focus:outline-none focus-visible:ring-2 focus-visible:ring-offset-2 focus-visible:ring-blue-500"
                    type="button"
                    @click="closeModal"
                >
                  Got it
                </button>
              </div>
            </div>
          </TransitionChild>
        </div>
      </div>
    </Dialog>
  </TransitionRoot>

</template>

<script>
import $ from "jquery";
import "jquery-ui/ui/widgets/draggable";
import "jquery-ui/ui/widgets/droppable";
import "jquery-ui/ui/widgets/sortable";
import "jquery-ui/ui/widgets/resizable";
//import "jquery-ui/ui/widgets/tooltip";
import "jquery-ui/themes/base/resizable.css";
//import "jquery-ui/themes/base/tooltip.css";
import CodeMirror from "codemirror/lib/codemirror";
import "codemirror/lib/codemirror.css";
import "codemirror/addon/selection/active-line";
import "codemirror/addon/selection/mark-selection";
import "codemirror/addon/selection/selection-pointer";
import "codemirror/addon/edit/matchbrackets";
import "codemirror/theme/monokai.css";
import "codemirror/mode/htmlmixed/htmlmixed";
import "codemirror/mode/javascript/javascript";
import "codemirror/mode/xml/xml";
import "codemirror/addon/scroll/simplescrollbars";
import "codemirror/addon/scroll/simplescrollbars.css";
import js_beautify from "js-beautify";
import {EditorState, EditorView, basicSetup} from "@codemirror/basic-setup"
//import {javascript} from "@codemirror/lang-javascript"
import {html} from "@codemirror/lang-html"
//import tippy from 'tippy.js/headless/dist/tippy-headless.cjs';
import tippy from 'tippy.js';
import 'tippy.js/dist/tippy.css';
import 'tippy.js/themes/light-border.css'

import {ref} from "vue";
import {
  Dialog,
  DialogOverlay,
  //DialogTitle,
  TransitionRoot,
  TransitionChild,
} from "@headlessui/vue";

export default {

  components: {
    Dialog,
    DialogOverlay,
    //DialogTitle,
    TransitionRoot,
    TransitionChild,
  },
  setup() {
    const isOpen = ref(false)

    return {
      isOpen,
      modal_message: '',

      closeModal() {
        isOpen.value = false
      },
      openModal() {
        isOpen.value = true
      },
    }
  },

  name: "MainFrame",
  data() {
    return {
      dragging_element_name: "",
      code_mirror_editor: null,
      show_code_mirror: false,
      codemirror_classes_editor: null,
      code_editor: null,
      selected_element: null,
      right_clicked_element: null,
      selected_element_html: "",
      selected_element_inner_html: "",
      selected_element_classes: "",

      margins: [
        "m-0", "m-px", "m-0.5", "m-1", "m-1.5", "m-2", "m-2.5", "m-3", "m-3.5", "m-4", "m-5",
        "m-6", "m-7", "m-8", "m-9", "m-10", "m-11", "m-12", "m-14", "m-16", "m-20", "m-24", "m-28",
        "m-32", "m-36", "m-40", "m-44", "m-48", "m-52", "m-56", "m-60", "m-64", "m-72", "m-80", "m-96",
        "m-auto", "-m-0", "-m-px", "-m-0.5", "-m-1", "-m-1.5", "-m-2", "-m-2.5", "-m-3", "-m-3.5", "-m-4", "-m-5",
        "-m-6", "-m-7", "-m-8", "-m-9", "-m-10", "-m-11", "-m-12", "-m-14", "-m-16", "-m-20", "-m-24", "-m-28",
        "-m-32", "-m-36", "-m-40", "-m-44", "-m-48", "-m-52", "-m-56", "-m-60",
        "-m-64", "-m-72", "-m-80", "-m-96", "mx-0", "mx-px", "mx-0.5", "mx-1",
        "mx-1.5", "mx-2", "mx-2.5", "mx-3", "mx-3.5", "mx-4", "mx-5", "mx-6", "mx-7", "mx-8", "mx-9", "mx-10",
        "mx-11", "mx-12", "mx-14", "mx-16", "mx-20", "mx-24", "mx-28", "mx-32",
        "mx-36", "mx-40", "mx-44", "mx-48", "mx-52", "mx-56", "mx-60", "mx-64",
        "mx-72", "mx-80", "mx-96", "mx-auto", "-mx-0", "-mx-px", "-mx-0.5", "-mx-1",
        "-mx-1.5", "-mx-2", "-mx-2.5", "-mx-3", "-mx-3.5", "-mx-4", "-mx-5", "-mx-6",
        "-mx-7", "-mx-8", "-mx-9", "-mx-10", "-mx-11", "-mx-12", "-mx-14", "-mx-16",
        "-mx-20", "-mx-24", "-mx-28", "-mx-32", "-mx-36", "-mx-40", "-mx-44", "-mx-48",
        "-mx-52", "-mx-56", "-mx-60", "-mx-64", "-mx-72", "-mx-80", "-mx-96", "my-0", "my-px", "my-0.5", "my-1", "my-1.5",
        "my-2", "my-2.5", "my-3", "my-3.5", "my-4", "my-5", "my-6", "my-7", "my-8", "my-9", "my-10", "my-11",
        "my-12", "my-14", "my-16", "my-20", "my-24", "my-28", "my-32", "my-36",
        "my-40", "my-44", "my-48", "my-52", "my-56", "my-60", "my-64", "my-72",
        "my-80", "my-96", "my-auto", "-my-0", "-my-px", "-my-0.5", "-my-1", "-my-1.5",
        "-my-2", "-my-2.5", "-my-3", "-my-3.5", "-my-4", "-my-5", "-my-6", "-my-7", "-my-8", "-my-9",
        "-my-10", "-my-11", "-my-12", "-my-14", "-my-16", "-my-20", "-my-24", "-my-28",
        "-my-32", "-my-36", "-my-40", "-my-44", "-my-48", "-my-52", "-my-56", "-my-60",
        "-my-64", "-my-72", "-my-80", "-my-96", "mt-0", "mt-px", "mt-0.5", "mt-1",
        "mt-1.5", "mt-2", "mt-2.5", "mt-3", "mt-3.5", "mt-4", "mt-5", "mt-6",
        "mt-7", "mt-8", "mt-9", "mt-10", "mt-11", "mt-12", "mt-14", "mt-16",
        "mt-20", "mt-24", "mt-28", "mt-32", "mt-36", "mt-40", "mt-44", "mt-48",
        "mt-52", "mt-56", "mt-60", "mt-64", "mt-72", "mt-80", "mt-96", "mt-auto",
        "-mt-0", "-mt-px", "-mt-0.5", "-mt-1", "-mt-1.5", "-mt-2", "-mt-2.5",
        "-mt-3", "-mt-3.5", "-mt-4", "-mt-5", "-mt-6", "-mt-7", "-mt-8", "-mt-9",
        "-mt-10", "-mt-11", "-mt-12", "-mt-14", "-mt-16", "-mt-20", "-mt-24", "-mt-28",
        "-mt-32", "-mt-36", "-mt-40", "-mt-44", "-mt-48", "-mt-52", "-mt-56", "-mt-60", "-mt-64", "-mt-72",
        "-mt-80", "-mt-96", "mr-0", "mr-px", "mr-0.5", "mr-1", "mr-1.5", "mr-2", "mr-2.5", "mr-3",
        "mr-3.5", "mr-4",
        "mr-5", "mr-6",
        "mr-7", "mr-8",
        "mr-9", "mr-10",
        "mr-11", "mr-12",
        "mr-14", "mr-16",
        "mr-20", "mr-24",
        "mr-28", "mr-32",
        "mr-36", "mr-40",
        "mr-44", "mr-48",
        "mr-52", "mr-56",
        "mr-60", "mr-64",
        "mr-72", "mr-80",
        "mr-96", "mr-auto",
        "-mr-0", "-mr-px",
        "-mr-0.5", "-mr-1",
        "-mr-1.5", "-mr-2",
        "-mr-2.5", "-mr-3",
        "-mr-3.5", "-mr-4",
        "-mr-5", "-mr-6",
        "-mr-7", "-mr-8",
        "-mr-9", "-mr-10",
        "-mr-11", "-mr-12",
        "-mr-14", "-mr-16",
        "-mr-20", "-mr-24",
        "-mr-28", "-mr-32",
        "-mr-36", "-mr-40",
        "-mr-44", "-mr-48",
        "-mr-52", "-mr-56",
        "-mr-60", "-mr-64",
        "-mr-72", "-mr-80",
        "-mr-96", "mb-0",
        "mb-px", "mb-0.5",
        "mb-1", "mb-1.5",
        "mb-2", "mb-2.5",
        "mb-3", "mb-3.5",
        "mb-4", "mb-5",
        "mb-6", "mb-7",
        "mb-8", "mb-9",
        "mb-10", "mb-11",
        "mb-12", "mb-14",
        "mb-16", "mb-20",
        "mb-24", "mb-28",
        "mb-32", "mb-36",
        "mb-40", "mb-44",
        "mb-48", "mb-52",
        "mb-56", "mb-60",
        "mb-64", "mb-72",
        "mb-80", "mb-96",
        "mb-auto", "-mb-0",
        "-mb-px", "-mb-0.5",
        "-mb-1", "-mb-1.5",
        "-mb-2", "-mb-2.5",
        "-mb-3",
        "-mb-3.5",
        "-mb-4",
        "-mb-5", "-mb-6",
        "-mb-7", "-mb-8",
        "-mb-9", "-mb-10",
        "-mb-11", "-mb-12",
        "-mb-14", "-mb-16",
        "-mb-20", "-mb-24",
        "-mb-28", "-mb-32",
        "-mb-36", "-mb-40",
        "-mb-44", "-mb-48",
        "-mb-52", "-mb-56",
        "-mb-60", "-mb-64",
        "-mb-72", "-mb-80",
        "-mb-96", "ml-0",
        "ml-px", "ml-0.5",
        "ml-1", "ml-1.5",
        "ml-2", "ml-2.5",
        "ml-3", "ml-3.5",
        "ml-4", "ml-5", "ml-6",
        "ml-7", "ml-8",
        "ml-9", "ml-10",
        "ml-11", "ml-12",
        "ml-14", "ml-16",
        "ml-20", "ml-24", "ml-28",
        "ml-32", "ml-36",
        "ml-40", "ml-44",
        "ml-48", "ml-52",
        "ml-56", "ml-60",
        "ml-64", "ml-72",
        "ml-80", "ml-96",
        "ml-auto", "-ml-0",
        "-ml-px", "-ml-0.5",
        "-ml-1", "-ml-1.5", "-ml-2",
        "-ml-2.5", "-ml-3",
        "-ml-3.5", "-ml-4",
        "-ml-5", "-ml-6", "-ml-7", "-ml-8", "-ml-9", "-ml-10", "-ml-11", "-ml-12", "-ml-14", "-ml-16", "-ml-20", "-ml-24", "-ml-28",
        "-ml-32", "-ml-36", "-ml-40", "-ml-44", "-ml-48", "-ml-52", "-ml-56", "-ml-60", "-ml-64", "-ml-72", "-ml-80", "-ml-96",
      ],
      paddings: [
        "p-0",
        "p-px",
        "p-0.5",
        "p-1",
        "p-1.5",
        "p-2",
        "p-2.5",
        "p-3",
        "p-3.5",
        "p-4",
        "p-5",
        "p-6",
        "p-7",
        "p-8",
        "p-9",
        "p-10",
        "p-11",
        "p-12",
        "p-14",
        "p-16",
        "p-20",
        "p-24",
        "p-28",
        "p-32",
        "p-36",
        "p-40",
        "p-44",
        "p-48",
        "p-52",
        "p-56",
        "p-60",
        "p-64",
        "p-72",
        "p-80",
        "p-96",
        "px-0",
        "px-px",
        "px-0.5",
        "px-1",
        "px-1.5",
        "px-2",
        "px-2.5",
        "px-3",
        "px-3.5",
        "px-4",
        "px-5",
        "px-6",
        "px-7",
        "px-8",
        "px-9",
        "px-10",
        "px-11",
        "px-12",
        "px-14",
        "px-16",
        "px-20",
        "px-24",
        "px-28",
        "px-32",
        "px-36",
        "px-40",
        "px-44",
        "px-48",
        "px-52",
        "px-56",
        "px-60",
        "px-64",
        "px-72",
        "px-80",
        "px-96",
        "py-0	",
        "py-px",
        "py-0.5",
        "py-1",
        "py-1.5",
        "py-2",
        "py-2.5",
        "py-3",
        "py-3.5",
        "py-4",
        "py-5",
        "py-6",
        "py-7",
        "py-8",
        "py-9",
        "py-10",
        "py-11",
        "py-12",
        "py-14",
        "py-16",
        "py-20",
        "py-24",
        "py-28",
        "py-32",
        "py-36",
        "py-40",
        "py-44",
        "py-48",
        "py-52",
        "py-56",
        "py-60",
        "py-64",
        "py-72",
        "py-80",
        "py-96",
        "pt-0",
        "pt-px",
        "pt-0.5",
        "pt-1",
        "pt-1.5",
        "pt-2",
        "pt-2.5",
        "pt-3",
        "pt-3.5",
        "pt-4",
        "pt-5",
        "pt-6",
        "pt-7",
        "pt-8",
        "pt-9",
        "pt-10",
        "pt-11",
        "pt-12",
        "pt-14",
        "pt-16",
        "pt-20",
        "pt-24",
        "pt-28",
        "pt-32",
        "pt-36",
        "pt-40",
        "pt-44",
        "pt-48",
        "pt-52",
        "pt-56",
        "pt-60",
        "pt-64",
        "pt-72",
        "pt-80",
        "pt-96",
        "pr-0",
        "pr-px",
        "pr-0.5",
        "pr-1",
        "pr-1.5",
        "pr-2",
        "pr-2.5",
        "pr-3",
        "pr-3.5",
        "pr-4",
        "pr-5",
        "pr-6",
        "pr-7",
        "pr-8",
        "pr-9",
        "pr-10",
        "pr-11",
        "pr-12",
        "pr-14",
        "pr-16",
        "pr-20",
        "pr-24",
        "pr-28",
        "pr-32",
        "pr-36",
        "pr-40",
        "pr-44",
        "pr-48",
        "pr-52",
        "pr-56",
        "pr-60",
        "pr-64",
        "pr-72",
        "pr-80",
        "pr-96",
        "pb-0",
        "pb-px",
        "pb-0.5",
        "pb-1",
        "pb-1.5",
        "pb-2",
        "pb-2.5",
        "pb-3",
        "pb-3.5",
        "pb-4",
        "pb-5",
        "pb-6",
        "pb-7",
        "pb-8",
        "pb-9",
        "pb-10",
        "pb-11",
        "pb-12",
        "pb-14",
        "pb-16",
        "pb-20",
        "pb-24",
        "pb-28",
        "pb-32",
        "pb-36",
        "pb-40",
        "pb-44",
        "pb-48",
        "pb-52",
        "pb-56",
        "pb-60",
        "pb-64",
        "pb-72",
        "pb-80",
        "pb-96",
        "pl-0",
        "pl-px",
        "pl-0.5",
        "pl-1",
        "pl-1.5",
        "pl-2",
        "pl-2.5",
        "pl-3",
        "pl-3.5",
        "pl-4",
        "pl-5",
        "pl-6",
        "pl-7",
        "pl-8",
        "pl-9",
        "pl-10",
        "pl-11",
        "pl-12",
        "pl-14",
        "pl-16",
        "pl-20",
        "pl-24",
        "pl-28",
        "pl-32",
        "pl-36",
        "pl-40",
        "pl-44",
        "pl-48",
        "pl-52",
        "pl-56",
        "pl-60",
        "pl-64",
        "pl-72",
        "pl-80",
        "pl-96"
      ],
      widths: ["w-0", "w-px", "w-0.5", "w-1", "w-1.5", "w-2", "w-2.5", "w-3", "w-3.5", "w-4",
        "w-5", "w-6", "w-7", "w-8", "w-9", "w-10", "w-11", "w-12", "w-14", "w-16", "w-20",
        "w-24", "w-28", "w-32", "w-36", "w-40", "w-44", "w-48", "w-52", "w-56", "w-60", "w-64",
        "w-72", "w-80", "w-96", "w-auto", "w-1/2", "w-1/3", "w-2/3", "w-1/4", "w-2/4", "w-3/4",
        "w-1/5", "w-2/5", "w-3/5", "w-4/5", "w-1/6", "w-2/6", "w-3/6", "w-4/6", "w-5/6", "w-1/12",
        "w-2/12", "w-3/12", "w-4/12", "w-5/12", "w-6/12", "w-7/12", "w-8/12", "w-9/12", "w-10/12",
        "w-11/12", "w-full", "w-screen", "w-min", "w-max"
      ],
      heights: [
        "h-0", "h-px", "h-0.5", "h-1", "h-1.5", "h-2", "h-2.5",
        "h-3", "h-3.5", "h-4", "h-5", "h-6", "h-7", "h-8", "h-9",
        "h-10", "h-11", "h-12", "h-14", "h-16", "h-20", "h-24", "h-28",
        "h-32", "h-36", "h-40", "h-44", "h-48", "h-52", "h-56",
        "h-60", "h-64", "h-72", "h-80", "h-96", "h-auto", "h-1/2",
        "h-1/3", "h-2/3", "h-1/4", "h-2/4", "h-3/4", "h-1/5",
        "h-2/5", "h-3/5", "h-4/5", "h-1/6", "h-2/6", "h-3/6",
        "h-4/6", "h-5/6", "h-full", "h-screen",
      ],
      bg_colors: [
        "bg-transparent", "bg-current", "bg-black", "bg-white",
        "bg-gray-50", "bg-gray-100", "bg-gray-200", "bg-gray-300", "bg-gray-400", "bg-gray-500", "bg-gray-600", "bg-gray-700", "bg-gray-800", "bg-gray-900",
        "bg-red-50", "bg-red-100", "bg-red-200", "bg-red-300", "bg-red-400", "bg-red-500", "bg-red-600", "bg-red-700", "bg-red-800", "bg-red-900",
        "bg-yellow-50", "bg-yellow-100", "bg-yellow-200", "bg-yellow-300", "bg-yellow-400", "bg-yellow-500", "bg-yellow-600", "bg-yellow-700", "bg-yellow-800", "bg-yellow-900",
        "bg-green-50", "bg-green-100", "bg-green-200", "bg-green-300", "bg-green-400", "bg-green-500", "bg-green-600", "bg-green-700", "bg-green-800", "bg-green-900",
        "bg-blue-50", "bg-blue-100", "bg-blue-200", "bg-blue-300", "bg-blue-400", "bg-blue-500", "bg-blue-600", "bg-blue-700", "bg-blue-800", "bg-blue-900",
        "bg-indigo-50", "bg-indigo-100", "bg-indigo-200", "bg-indigo-300", "bg-indigo-400", "bg-indigo-500", "bg-indigo-600", "bg-indigo-700", "bg-indigo-800", "bg-indigo-900",
        "bg-purple-50", "bg-purple-100", "bg-purple-200", "bg-purple-300", "bg-purple-400", "bg-purple-500", "bg-purple-600", "bg-purple-700", "bg-purple-800", "bg-purple-900",
        "bg-pink-50", "bg-pink-100", "bg-pink-200", "bg-pink-300", "bg-pink-400", "bg-pink-500", "bg-pink-600", "bg-pink-700", "bg-pink-800", "bg-pink-900",
      ],
      text_colors: [
        "text-transparent", "text-current", "text-black", "text-white",
        "text-gray-50", "text-gray-100", "text-gray-200", "text-gray-300", "text-gray-400", "text-gray-500", "text-gray-600", "text-gray-700", "text-gray-800", "text-gray-900",
        "text-red-50", "text-red-100", "text-red-200", "text-red-300", "text-red-400", "text-red-500", "text-red-600", "text-red-700", "text-red-800", "text-red-900",
        "text-yellow-50", "text-yellow-100", "text-yellow-200", "text-yellow-300", "text-yellow-400", "text-yellow-500", "text-yellow-600", "text-yellow-700", "text-yellow-800", "text-yellow-900",
        "text-green-50", "text-green-100", "text-green-200", "text-green-300", "text-green-400", "text-green-500", "text-green-600", "text-green-700", "text-green-800", "text-green-900",
        "text-blue-50", "text-blue-100", "text-blue-200", "text-blue-300", "text-blue-400", "text-blue-500", "text-blue-600", "text-blue-700", "text-blue-800", "text-blue-900",
        "text-indigo-50", "text-indigo-100", "text-indigo-200", "text-indigo-300", "text-indigo-400", "text-indigo-500", "text-indigo-600", "text-indigo-700", "text-indigo-800", "text-indigo-900",
        "text-purple-50", "text-purple-100", "text-purple-200", "text-purple-300", "text-purple-400", "text-purple-500", "text-purple-600", "text-purple-700", "text-purple-800", "text-purple-900",
        "text-pink-50", "text-pink-100", "text-pink-200", "text-pink-300", "text-pink-400", "text-pink-500", "text-pink-600", "text-pink-700", "text-pink-800", "text-pink-900",

      ],
      text_sizes: [
        "text-xs", "text-sm", "text-base", "text-lg",
        "text-xl", "text-2xl", "text-3xl", "text-4xl", "text-5xl", "text-6xl",
        "text-7xl", "text-8xl", "text-9xl"
      ],
      font_families: [
        "font-sans", "font-serif", "font-mono"
      ],
      font_weights: [
        "font-thin", "font-extralight", "font-light", "font-normal",
        "font-medium", "font-semibold", "font-bold", "font-extrabold", "font-black",
      ],
      border_colors: [
        "border-transparent", "border-current", "border-black", "border-white",
        "border-gray-50", "border-gray-100", "border-gray-200", "border-gray-300", "border-gray-400", "border-gray-500", "border-gray-600", "border-gray-700", "border-gray-800", "border-gray-900",
        "border-red-50", "border-red-100", "border-red-200", "border-red-300", "border-red-400", "border-red-500", "border-red-600", "border-red-700", "border-red-800", "border-red-900",
        "border-yellow-50", "border-yellow-100", "border-yellow-200", "border-yellow-300", "border-yellow-400", "border-yellow-500", "border-yellow-600", "border-yellow-700", "border-yellow-800", "border-yellow-900",
        "border-green-50", "border-green-100", "border-green-200", "border-green-300", "border-green-400", "border-green-500", "border-green-600", "border-green-700", "border-green-800", "border-green-900",
        "border-blue-50", "border-blue-100", "border-blue-200", "border-blue-300", "border-blue-400", "border-blue-500", "border-blue-600", "border-blue-700", "border-blue-800", "border-blue-900",
        "border-indigo-50", "border-indigo-100", "border-indigo-200", "border-indigo-300", "border-indigo-400", "border-indigo-500", "border-indigo-600", "border-indigo-700", "border-indigo-800", "border-indigo-900",
        "border-purple-50", "border-purple-100", "border-purple-200", "border-purple-300", "border-purple-400", "border-purple-500", "border-purple-600", "border-purple-700", "border-purple-800", "border-purple-900",
        "border-pink-50", "border-pink-100", "border-pink-200", "border-pink-300", "border-pink-400", "border-pink-500", "border-pink-600", "border-pink-700", "border-pink-800", "border-pink-900",
      ],
      border_widths: [
        "border-0", "border-2", "border-4", "border-8", "border", "border-t-0",
        "border-t-2", "border-t-4", "border-t-8", "border-t	", "border-r-0	",
        "border-r-2", "border-r-4", "border-r-8", "border-r", "border-b-0",
        "border-b-2", "border-b-4", "border-b-8", "border-b", "border-l-0", "border-l-2", "border-l-4",
        "border-l-8", "border-l",
      ],
      border_styles: [
        "border-solid", "border-dashed", "border-dotted", "border-double", "border-none"
      ],
      border_radiuses: [
        "rounded-none", "rounded-sm", "rounded", "rounded-md", "rounded-lg",
        "rounded-xl", "rounded-2xl", "rounded-3xl", "rounded-full", "rounded-t-none",
        "rounded-t-sm", "rounded-t", "rounded-t-md", "rounded-t-lg", "rounded-t-xl",
        "rounded-t-2xl", "rounded-t-3xl", "rounded-t-full", "rounded-r-none",
        "rounded-r-sm", "rounded-r", "rounded-r-md",
        "rounded-r-lg", "rounded-r-xl", "rounded-r-2xl", "rounded-r-3xl", "rounded-r-full",
        "rounded-b-none", "rounded-b-sm", "rounded-b", "rounded-b-md", "rounded-b-lg",
        "rounded-b-xl", "rounded-b-2xl", "rounded-b-3xl", "rounded-b-full", "rounded-l-none",
        "rounded-l-sm", "rounded-l", "rounded-l-md", "rounded-l-lg", "rounded-l-xl", "rounded-l-2xl",
        "rounded-l-3xl", "rounded-l-full", "rounded-tl-none",
        "rounded-tl-sm", "rounded-tl",
        "rounded-tl-md", "rounded-tl-lg", "rounded-tl-xl",
        "rounded-tl-2xl", "rounded-tl-3xl", "rounded-tl-full",
        "rounded-tr-none", "rounded-tr-sm", "rounded-tr", "rounded-tr-md",
        "rounded-tr-lg", "rounded-tr-xl", "rounded-tr-2xl", "rounded-tr-3xl",
        "rounded-tr-full", "rounded-br-none", "rounded-br-sm",
        "rounded-br", "rounded-br-md", "rounded-br-lg", "rounded-br-xl", "rounded-br-2xl",
        "rounded-br-3xl", "rounded-br-full", "rounded-bl-none", "rounded-bl-sm", "rounded-bl",
        "rounded-bl-md", "rounded-bl-lg", "rounded-bl-xl", "rounded-bl-2xl",
        "rounded-bl-3xl", "rounded-bl-full"
      ],
      ring_colors: [
        "ring-transparent", "ring-current", "ring-black", "ring-white",
        "ring-gray-50", "ring-gray-100", "ring-gray-200", "ring-gray-300", "ring-gray-400", "ring-gray-500", "ring-gray-600", "ring-gray-700", "ring-gray-800", "ring-gray-900",
        "ring-red-50", "ring-red-100", "ring-red-200", "ring-red-300", "ring-red-400", "ring-red-500", "ring-red-600", "ring-red-700", "ring-red-800", "ring-red-900",
        "ring-yellow-50", "ring-yellow-100", "ring-yellow-200", "ring-yellow-300", "ring-yellow-400", "ring-yellow-500", "ring-yellow-600", "ring-yellow-700", "ring-yellow-800", "ring-yellow-900",
        "ring-green-50", "ring-green-100", "ring-green-200", "ring-green-300", "ring-green-400", "ring-green-500", "ring-green-600", "ring-green-700", "ring-green-800", "ring-green-900",
        "ring-blue-50", "ring-blue-100", "ring-blue-200", "ring-blue-300", "ring-blue-400", "ring-blue-500", "ring-blue-600", "ring-blue-700", "ring-blue-800", "ring-blue-900",
        "ring-indigo-50", "ring-indigo-100", "ring-indigo-200", "ring-indigo-300", "ring-indigo-400", "ring-indigo-500", "ring-indigo-600", "ring-indigo-700", "ring-indigo-800", "ring-indigo-900",
        "ring-purple-50", "ring-purple-100", "ring-purple-200", "ring-purple-300", "ring-purple-400", "ring-purple-500", "ring-purple-600", "ring-purple-700", "ring-purple-800", "ring-purple-900",
        "ring-pink-50", "ring-pink-100", "ring-pink-200", "ring-pink-300", "ring-pink-400", "ring-pink-500", "ring-pink-600", "ring-pink-700", "ring-pink-800", "ring-pink-900",
      ],
      shadows: [
        /*"*, ::before, ::after",*/ "shadow-sm", "box-shadow", "shadow",
        "box-shadow", "shadow-md", "box-shadow", "shadow-lg", "box-shadow",
        "shadow-xl", "box-shadow", "shadow-2xl", "box-shadow", "shadow-inner",
        "box-shadow", "shadow-none"
      ],
      flexes: [
        "flex-row", "flex-row-reverse", "flex-col", "flex-col-reverse"
      ],
      positions: ["static", "fixed", "absolute", "relative", "sticky"],
      trblis: [
        "inset-0", "-inset-0",
        "inset-x-0",
        "-inset-x-0",
        "inset-y-0",
        "-inset-y-0",
        "top-0",
        "-top-0",
        "right-0",
        "-right-0",
        "bottom-0",
        "-bottom-0",
        "left-0",
        "-left-0",
        "inset-px",
        "-inset-px",
        "inset-x-px",
        "-inset-x-px",
        "inset-y-px",
        "-inset-y-px",
        "top-px",
        "-top-px",
        "right-px",
        "-right-px",
        "bottom-px",
        "-bottom-px",
        "left-px",
        "-left-px",
        "inset-0.5",
        "-inset-0.5",
        "inset-x-0.5",
        "-inset-x-0.5",
        "inset-y-0.5",
        "-inset-y-0.5",
        "top-0.5",
        "-top-0.5",
        "right-0.5",
        "-right-0.5",
        "bottom-0.5",
        "-bottom-0.5",
        "left-0.5",
        "-left-0.5",
        "inset-1",
        "-inset-1",
        "inset-x-1",
        "-inset-x-1",
        "inset-y-1",
        "-inset-y-1",
        "top-1",
        "-top-1",
        "right-1",
        "-right-1",
        "bottom-1",
        "-bottom-1",
        "left-1",
        "-left-1",
        "inset-1.5",
        "-inset-1.5",
        "inset-x-1.5",
        "-inset-x-1.5",
        "inset-y-1.5",
        "-inset-y-1.5",
        "top-1.5",
        "-top-1.5",
        "right-1.5",
        "-right-1.5",
        "bottom-1.5",
        "-bottom-1.5",
        "left-1.5",
        "-left-1.5",
        "inset-2",
        "-inset-2",
        "inset-x-2",
        "-inset-x-2",
        "inset-y-2",
        "-inset-y-2",
        "top-2",
        "-top-2",
        "right-2",
        "-right-2",
        "bottom-2",
        "-bottom-2",
        "left-2",
        "-left-2",
        "inset-2.5",
        "-inset-2.5",
        "inset-x-2.5",
        "-inset-x-2.5",
        "inset-y-2.5",
        "-inset-y-2.5",
        "top-2.5",
        "-top-2.5",
        "right-2.5",
        "-right-2.5",
        "bottom-2.5",
        "-bottom-2.5",
        "left-2.5",
        "-left-2.5",
        "inset-3",
        "-inset-3",
        "inset-x-3",
        "-inset-x-3",
        "inset-y-3",
        "-inset-y-3",
        "top-3",
        "-top-3",
        "right-3",
        "-right-3",
        "bottom-3",
        "-bottom-3",
        "left-3",
        "-left-3",
        "inset-3.5",
        "-inset-3.5",
        "inset-x-3.5",
        "-inset-x-3.5",
        "inset-y-3.5",
        "-inset-y-3.5",
        "top-3.5",
        "-top-3.5",
        "right-3.5",
        "-right-3.5",
        "bottom-3.5",
        "-bottom-3.5",
        "left-3.5",
        "-left-3.5",
        "inset-4",
        "-inset-4",
        "inset-x-4",
        "-inset-x-4",
        "inset-y-4",
        "-inset-y-4",
        "top-4",
        "-top-4",
        "right-4",
        "-right-4",
        "bottom-4",
        "-bottom-4",
        "left-4",
        "-left-4",
        "inset-5",
        "-inset-5",
        "inset-x-5",
        "-inset-x-5",
        "inset-y-5",
        "-inset-y-5",
        "top-5",
        "-top-5",
        "right-5",
        "-right-5",
        "bottom-5",
        "-bottom-5",
        "left-5",
        "-left-5",
        "inset-6",
        "-inset-6",
        "inset-x-6",
        "-inset-x-6",
        "inset-y-6",
        "-inset-y-6",
        "top-6",
        "-top-6",
        "right-6",
        "-right-6",
        "bottom-6",
        "-bottom-6",
        "left-6",
        "-left-6",
        "inset-7",
        "-inset-7",
        "inset-x-7",
        "-inset-x-7",
        "inset-y-7",
        "-inset-y-7",
        "top-7",
        "-top-7",
        "right-7",
        "-right-7",
        "bottom-7",
        "-bottom-7",
        "left-7",
        "-left-7",
        "inset-8",
        "-inset-8",
        "inset-x-8",
        "-inset-x-8",
        "inset-y-8",
        "-inset-y-8",
        "top-8",
        "-top-8",
        "right-8",
        "-right-8",
        "bottom-8",
        "-bottom-8",
        "left-8",
        "-left-8",
        "inset-9",
        "-inset-9",
        "inset-x-9",
        "-inset-x-9",
        "inset-y-9",
        "-inset-y-9",
        "top-9",
        "-top-9",
        "right-9",
        "-right-9",
        "bottom-9",
        "-bottom-9",
        "left-9",
        "-left-9",
        "inset-10",
        "-inset-10",
        "inset-x-10",
        "-inset-x-10",
        "inset-y-10",
        "-inset-y-10",
        "top-10",
        "-top-10",
        "right-10",
        "-right-10",
        "bottom-10",
        "-bottom-10",
        "left-10",
        "-left-10",
        "inset-11",
        "-inset-11",
        "inset-x-11",
        "-inset-x-11",
        "inset-y-11",
        "-inset-y-11",
        "top-11",
        "-top-11",
        "right-11",
        "-right-11",
        "bottom-11",
        "-bottom-11",
        "left-11",
        "-left-11",
        "inset-12",
        "-inset-12",
        "inset-x-12",
        "-inset-x-12",
        "inset-y-12",
        "-inset-y-12",
        "top-12",
        "-top-12",
        "right-12",
        "-right-12",
        "bottom-12",
        "-bottom-12",
        "left-12",
        "-left-12",
        "inset-14",
        "-inset-14",
        "inset-x-14",
        "-inset-x-14",
        "inset-y-14",
        "-inset-y-14",
        "top-14",
        "-top-14",
        "right-14",
        "-right-14",
        "bottom-14",
        "-bottom-14",
        "left-14",
        "-left-14",
        "inset-16",
        "-inset-16",
        "inset-x-16",
        "-inset-x-16",
        "inset-y-16",
        "-inset-y-16",
        "top-16",
        "-top-16",
        "right-16",
        "-right-16",
        "bottom-16",
        "-bottom-16",
        "left-16",
        "-left-16",
        "inset-20",
        "-inset-20",
        "inset-x-20",
        "-inset-x-20",
        "inset-y-20",
        "-inset-y-20",
        "top-20",
        "-top-20",
        "right-20",
        "-right-20",
        "bottom-20",
        "-bottom-20",
        "left-20",
        "-left-20",
        "inset-24",
        "-inset-24",
        "inset-x-24",
        "-inset-x-24",
        "inset-y-24",
        "-inset-y-24",
        "top-24",
        "-top-24",
        "right-24",
        "-right-24",
        "bottom-24",
        "-bottom-24",
        "left-24",
        "-left-24",
        "inset-28",
        "-inset-28",
        "inset-x-28",
        "-inset-x-28",
        "inset-y-28",
        "-inset-y-28",
        "top-28",
        "-top-28",
        "right-28",
        "-right-28",
        "bottom-28",
        "-bottom-28",
        "left-28",
        "-left-28",
        "inset-32",
        "-inset-32",
        "inset-x-32",
        "-inset-x-32",
        "inset-y-32",
        "-inset-y-32",
        "top-32",
        "-top-32",
        "right-32",
        "-right-32",
        "bottom-32",
        "-bottom-32",
        "left-32",
        "-left-32",
        "inset-36",
        "-inset-36",
        "inset-x-36",
        "-inset-x-36",
        "inset-y-36",
        "-inset-y-36",
        "top-36",
        "-top-36",
        "right-36",
        "-right-36",
        "bottom-36",
        "-bottom-36",
        "left-36",
        "-left-36",
        "inset-40",
        "-inset-40",
        "inset-x-40",
        "-inset-x-40",
        "inset-y-40",
        "-inset-y-40",
        "top-40",
        "-top-40",
        "right-40",
        "-right-40",
        "bottom-40",
        "-bottom-40",
        "left-40",
        "-left-40",
        "inset-44",
        "-inset-44",
        "inset-x-44",
        "-inset-x-44",
        "inset-y-44",
        "-inset-y-44",
        "top-44",
        "-top-44",
        "right-44",
        "-right-44",
        "bottom-44",
        "-bottom-44",
        "left-44",
        "-left-44",
        "inset-48",
        "-inset-48",
        "inset-x-48",
        "-inset-x-48",
        "inset-y-48",
        "-inset-y-48",
        "top-48",
        "-top-48",
        "right-48",
        "-right-48",
        "bottom-48",
        "-bottom-48",
        "left-48",
        "-left-48",
        "inset-52",
        "-inset-52",
        "inset-x-52",
        "-inset-x-52",
        "inset-y-52",
        "-inset-y-52",
        "top-52",
        "-top-52",
        "right-52",
        "-right-52",
        "bottom-52",
        "-bottom-52",
        "left-52",
        "-left-52",
        "inset-56",
        "-inset-56",
        "inset-x-56",
        "-inset-x-56",
        "inset-y-56",
        "-inset-y-56",
        "top-56",
        "-top-56",
        "right-56",
        "-right-56",
        "bottom-56",
        "-bottom-56",
        "left-56",
        "-left-56",
        "inset-60",
        "-inset-60",
        "inset-x-60",
        "-inset-x-60",
        "inset-y-60",
        "-inset-y-60",
        "top-60",
        "-top-60",
        "right-60",
        "-right-60",
        "bottom-60",
        "-bottom-60",
        "left-60",
        "-left-60",
        "inset-64",
        "-inset-64",
        "inset-x-64",
        "-inset-x-64",
        "inset-y-64",
        "-inset-y-64",
        "top-64",
        "-top-64",
        "right-64",
        "-right-64",
        "bottom-64",
        "-bottom-64",
        "left-64",
        "-left-64",
        "inset-72",
        "-inset-72",
        "inset-x-72",
        "-inset-x-72",
        "inset-y-72",
        "-inset-y-72",
        "top-72",
        "-top-72",
        "right-72",
        "-right-72",
        "bottom-72",
        "-bottom-72",
        "left-72",
        "-left-72",
        "inset-80",
        "-inset-80",
        "inset-x-80",
        "-inset-x-80",
        "inset-y-80",
        "-inset-y-80",
        "top-80",
        "-top-80",
        "right-80",
        "-right-80",
        "bottom-80",
        "-bottom-80",
        "left-80",
        "-left-80",
        "inset-96",
        "-inset-96",
        "inset-x-96",
        "-inset-x-96",
        "inset-y-96",
        "-inset-y-96",
        "top-96",
        "-top-96",
        "right-96",
        "-right-96",
        "bottom-96",
        "-bottom-96",
        "left-96",
        "-left-96",
        "inset-auto",
        "inset-1/2",
        "inset-1/3",
        "inset-2/3",
        "inset-1/4",
        "inset-2/4",
        "inset-3/4",
        "inset-full",
        "-inset-1/2",
        "-inset-1/3",
        "-inset-2/3",
        "-inset-1/4",
        "-inset-2/4",
        "-inset-3/4",
        "-inset-full",
        "inset-x-auto",
        "inset-x-1/2",
        "inset-x-1/3",
        "inset-x-2/3",
        "inset-x-1/4",
        "inset-x-2/4",
        "inset-x-3/4",
        "inset-x-full",
        "-inset-x-1/2",
        "-inset-x-1/3",
        "-inset-x-2/3",
        "-inset-x-1/4",
        "-inset-x-2/4",
        "-inset-x-3/4",
        "-inset-x-full",
        "inset-y-auto",
        "inset-y-1/2",
        "inset-y-1/3",
        "inset-y-2/3",
        "inset-y-1/4",
        "inset-y-2/4",
        "inset-y-3/4",
        "inset-y-full",
        "-inset-y-1/2",
        "-inset-y-1/3",
        "-inset-y-2/3",
        "-inset-y-1/4",
        "-inset-y-2/4",
        "-inset-y-3/4",
        "-inset-y-full",
        "top-auto",
        "top-1/2",
        "top-1/3",
        "top-2/3",
        "top-1/4",
        "top-2/4",
        "top-3/4",
        "top-full",
        "-top-1/2",
        "-top-1/3",
        "-top-2/3",
        "-top-1/4",
        "-top-2/4",
        "-top-3/4",
        "-top-full",
        "right-auto",
        "right-1/2",
        "right-1/3",
        "right-2/3",
        "right-1/4",
        "right-2/4",
        "right-3/4",
        "right-full",
        "-right-1/2",
        "-right-1/3",
        "-right-2/3",
        "-right-1/4",
        "-right-2/4",
        "-right-3/4",
        "-right-full",
        "bottom-auto",
        "bottom-1/2",
        "bottom-1/3",
        "bottom-2/3",
        "bottom-1/4",
        "bottom-2/4",
        "bottom-3/4",
        "bottom-full",
        "-bottom-1/2",
        "-bottom-1/3",
        "-bottom-2/3",
        "-bottom-1/4",
        "-bottom-2/4",
        "-bottom-3/4",
        "-bottom-full",
        "left-auto",
        "left-1/2",
        "left-1/3",
        "left-2/3",
        "left-1/4",
        "left-2/4",
        "left-3/4",
        "left-full",
        "-left-1/2",
        "-left-1/3",
        "-left-2/3",
        "-left-1/4",
        "-left-2/4",
        "-left-3/4",
        "-left-full"
      ]
    };
  },
  methods: {
    apply_style() {
      console.log('apply style')
    },
    randomImage(){
      return "https://source.unsplash.com/random"
    },
    remove_class(class_name){
      if (this.selected_element === null) {
        this.modal_message = 'Click and select an element inside the canvas to apply design.'
        this.openModal()
        return;
      }

      $(this.selected_element).removeClass(class_name)

      this.selected_element_html = $(this.selected_element).prop("outerHTML")
      this.selected_element_classes = $(this.selected_element).attr("class")
    },
    add_class(event) {
      if (this.selected_element === null) {
        this.modal_message = 'Click and select an element inside the canvas to apply design.'
        this.openModal()
        return;
      }

      let class_to_add = $(event.currentTarget).val();
      let classes = $(this.selected_element).attr("class"); //.split(/\s+/)

      let style_to_apply = $(event.currentTarget).attr("id");

      if (/pt|pr|pb|pl/gim.test(style_to_apply)) {
        classes += " " + class_to_add;
      }
      else if (/mt|mr|mb|ml/gim.test(style_to_apply)) {
        classes += " " + class_to_add;
      }
      else if (style_to_apply === "position-type") {
        for (let index in this.positions) {
          let re = new RegExp(this.positions[index], 'gim');
          classes = classes.replace(re, '');
        }
        classes += " " + class_to_add;
      }
      else if (/position-(inset|top|right|bottom|left)/g.test(style_to_apply)) {
        let regex_data = $(event.currentTarget).attr("data-regex")

        for (let index in this.trblis) {
          if(new RegExp(regex_data).test(this.trblis[index])) {
            let re = new RegExp(this.trblis[index], 'gim');
            classes = classes.replace(re, '');
          }
        }
        classes += " " + class_to_add;
      }
      // if id="text-font"
      else if (/text-font/g.test(style_to_apply)) {
          for (let index in this.font_families) {
            classes = classes.replace(new RegExp(this.font_families[index], 'gim'), "");
          }

          classes += " " + class_to_add;
      }
      else if(/text-size/.test(style_to_apply)){
        for (let index in this.text_sizes) {
          classes = classes.replace(new RegExp(this.text_sizes[index], 'gim'), "");
        }

        classes += " " + class_to_add;
      }
      else if (/text-weight/g.test(style_to_apply)) {
        for (let index in this.font_weights) {
          classes = classes.replace(new RegExp(this.font_weights[index], 'gim'), "");
        }

        classes += " " + class_to_add;
      }
      else if (style_to_apply === "mt") {
        //classes = classes.replace(/m-\d+/gim, class_to_add);
        classes = classes.replace(/mt-\d+/gim, class_to_add);

        if (classes.indexOf(class_to_add) === -1) {
          classes += " " + class_to_add + " ";
        }
      }
      else if (style_to_apply === "bg") {
        classes = classes.replace(/bg-\w+(-\d+)?/gim, class_to_add);

        if (classes.indexOf(class_to_add) === -1) {
          classes += " " + class_to_add + " ";
        }
      } else if (style_to_apply === "text-color") {
        classes = classes.replace(/(^|\s)text-\w+-\d+/gim, " " + class_to_add + " ");

        if (classes.indexOf(class_to_add) === -1) {
          classes += " " + class_to_add + " ";
        }
      } else if (style_to_apply === "border-color") {
        classes = classes.replace(/(^|\s)border-\w+(-\d+)?/gim, " " + class_to_add + " ");

        if (classes.indexOf(class_to_add) === -1) {
          classes += " " + class_to_add + " ";
        }
      } else if (style_to_apply === "border-width") {
        //classes = classes.replace(/(^|\s)border-?[trbl]?-?\d?/gim, " " + class_to_add + " ");
        //let elem_classes = $(this.selected_element).attr("class").split(/\s+/)

        for (let border_width in this.border_widths) {
          classes.replace(/this.border_widths[border_width]/gim, '');
          console.log(this.border_widths[border_width])
        }

        classes += " " + class_to_add;
      } else if (style_to_apply === "border-style") {
        classes = classes.replace(/(^|\s)border-\w+/gim, " " + class_to_add + " ");

        if (classes.indexOf(class_to_add) === -1) {
          classes += " " + class_to_add + " ";
        }
      } else if (style_to_apply === "ring-color") {
        classes = classes.replace(/(^|\s)ring-\w+-\d+/gim, " " + class_to_add + " ");

        if (classes.indexOf(class_to_add) === -1) {
          classes += " " + class_to_add + " ";
        }
      } else if (style_to_apply === "flex-row-or-col") {
        classes = classes.replace(/(^|\s)flex-(col|row)/gim, " " + class_to_add + " ");

        if (classes.indexOf(class_to_add) === -1) {
          classes += " " + class_to_add + " ";
        }
      } else if (style_to_apply === "w") {
        classes = classes.replace(/(^|\s)w-\d+(\/?\d)?/gim, " " + class_to_add + " ");

        if (classes.indexOf(class_to_add) === -1) {
          classes += " " + class_to_add + " ";
        }
      } else if (style_to_apply === "h") {
        classes = classes.replace(/(^|\s)h-\d+(\/?\d)?/gim, " " + class_to_add + " ");

        if (classes.indexOf(class_to_add) === -1) {
          classes += " " + class_to_add + " ";
        }
      } else if (style_to_apply === "shadow") {
        classes = classes.replace(/(^|\s)(box-)?shadow-?(\w+)?/gim, " " + class_to_add + " ");

        if (classes.indexOf(class_to_add) === -1) {
          classes += " " + class_to_add + " ";
        }
      } else if (style_to_apply === "rounded") {
        classes = classes.replace(/(^|\s)rounded-?(\d+|\w+)?-?(\d+|\w+)?(\d+|\w+)?/gim, " " + class_to_add + " ");

        if (classes.indexOf(class_to_add) === -1) {
          classes += " " + class_to_add + " ";
        }
      }
      else if(style_to_apply === "add-class"){
        /*if (classes.indexOf(class_to_add) === -1) {
          classes += " " + class_to_add + " ";
        }*/
        classes += " " + class_to_add + " ";
        $(event.currentTarget).val('')
      }

      classes = classes.replace(/\s+/gim, " ");
      console.log(class_to_add, classes);
      $(this.selected_element).attr("class", classes);

      this.selected_element_html = $(this.selected_element).prop("outerHTML")
      this.selected_element_classes = classes //$(this.selected_element).attr("class")
    },
    add_element(event){
      if (this.selected_element === null) {
        //alert("To apply designs select one element by right clicking.");
        this.modal_message = 'Click and select an element inside the canvas to add element.'
        this.openModal()
        return;
      }

      let element_id = $(event.currentTarget).attr("id");
      this.dragging_element_name = element_id

      $(this.selected_element).append(this.create_dropped_element())
    },

    //let new_classes = ""
    /* $.each(classes, function(item) {
    if (item === 'someClass') {
        //
    }
}); */

    /* if(! $(this.selected_element).hasClass(class_to_add)){
        $(this.selected_element).addClass(class_to_add)
      } */

    clear_canvas() {
      $("#draggable-items-container").html("")
      $("#code-editor").html("").text("")
    },
    element_html_change() {
      if (!/<(?=.*? .*?\/ ?>|br|hr|input|!--|wbr)[a-z]+.*?>|<([a-z]+).*?<\/\1>/.test(this.selected_element_html)) {
        return
      }
      console.log('element_html_change', this.selected_element_html)
      let new_elem = $(this.selected_element_html)
      new_elem.insertAfter($(this.selected_element))
      $(this.selected_element).remove()
      this.selected_element = new_elem
    },
    duplicate_element() {
      let new_elem = $(this.selected_element).clone(true).removeClass('selected-element')
      //$(this.selected_element).clone(true).appendTo($(this.selected_element).parent())
      new_elem.insertAfter($(this.selected_element))
    },
    delete_element: function () {
      $(this.selected_element).remove()
    },
    remove_frame_border() {
      //$(this.selected_element).removeClass('container-frame')
    },
    create_dropped_element() {
      let self = this

      let element;
      if (self.dragging_element_name === "input") {
        element = document.createElement("input");
        $(element).addClass("p-2 border rounded-md");
      } else if (self.dragging_element_name === "text") {
        element = document.createElement("p");
        $(element)
            .text("You have just dropped a text element. Edit text by selecting this element.")
            .addClass("text-gray-600");
      } else if (self.dragging_element_name === "button") {
        element = document.createElement("button");
        $(element)
            .text("Button")
            .addClass("bg-black text-white py-2 px-4 rounded-md")
        //.attr('title', 'BUTTON Element')
      } else if (self.dragging_element_name === "img") {
        element = document.createElement("img");
        $(element).attr('src', self.randomImage())
            .addClass("w-64 rounded-md");
      } else if (self.dragging_element_name === "flex") {
        element = document.createElement("div");
        $(element)
            .addClass(
                "p-4 flex flex-col border border-gray-200 droppable-flex-container"
            )
            /* .droppable({
              accept: ".draggable",
              classes: {
                "ui-droppable-hover": "bg-indigo-100 border-dotted",
              },
              greedy: true,
              drop: function(event, ui) {
                console.log("flex drop");
                console.log(event);
                console.log(ui);},
            }) */
            .sortable({
              greedy: true,
              cancel: false,
              placeholder: "container-frame",
              //connectWith: ".connectedSortable"
              beforeStop: function (event, ui) {

                console.log("beforeStop", event, ui);
                console.log($(ui.item).hasClass("draggable"))

                if (!$(ui.item).hasClass("draggable")) { //self.dragging_element_name === ''
                  return
                }

                $(ui.item).replaceWith(self.create_dropped_element());

                self.code_mirror_editor.dispatch({
                  changes: {
                    from: 0,
                    to: self.code_mirror_editor.state.doc.toString().length,
                    insert: js_beautify.html($("#draggable-items-container").html(), {
                      "indent_size": "4",
                      "indent_char": " ",
                      "max_preserve_newlines": "5",
                      "preserve_newlines": true,
                    })
                  }
                })

                //$("#code-editor").text(js_beautify.html($("#draggable-items-container").html()))
              }
            })
        //.resizable();
      } else if (self.dragging_element_name === "grid") {
        element = document.createElement("div");
        $(element)
            .addClass("grid grid-cols-3 gap-4 p-2 border")
            .droppable({
              drop: function () {
                console.log("grid drop");
              },
            });
      }

      //$(this).append(this.create_dropped_element(self.dragging_element_name)); //element
      $(element).contextmenu(function () {
        if (self.right_clicked_element === null) {
          self.right_clicked_element = element

          self.modal_message = 'Click an element inside the canvas to add inside this element. Right click again to revert.'
          self.openModal()
        }
        else{
          self.right_clicked_element = null
        }
        return false;
      });

      $(element).click(function (e) {
        self.selected_element = e.target
        //console.log("left clicked", $(e.target).attr("class"));
        $("#element").text( $(e.target).prop('tagName') ).removeClass("hidden")
        //$("#element-html").val($(e.target).prop('outerHTML'))
        self.selected_element_html = $(e.target).prop('outerHTML')

        $("#draggable-items-container").find('*')/*.children()*/.removeClass('selected-element')
        $(e.target).addClass('selected-element').removeClass('container-frame')
        setTimeout(function () {
          $(e.target).removeClass('selected-element')
        }, 1000)

        self.selected_element_classes = $(e.target).attr('class')

        if (self.right_clicked_element !== null) {
          $(self.right_clicked_element).append(e.target)
          self.right_clicked_element = null
        }

        return false;
      });

      $(element).hover(function (){
        $(element).addClass('container-frame')
      }).mouseleave(function(){
        $(element).removeClass('container-frame')
      })

      /*tippy(element, {
        //content: $(element).prop('outerHTML'),
        content: $('#template').html(),
        interactive: true,
        trigger: 'click',
        allowHTML: true,
        //hideOnClick: 'toggle',
      });*/

      return element
    },
    get_code() {
      let self = this
      self.code_mirror_editor.dispatch({
        changes: {
          from: 0,
          to: self.code_mirror_editor.state.doc.toString().length,
          insert: js_beautify.html($("#draggable-items-container").html(), {
            "indent_size": "4",
            "indent_char": " ",
            "max_preserve_newlines": "5",
            "preserve_newlines": true,
          })
        }
      })
      this.show_code_mirror = !this.show_code_mirror
    }
  },
  mounted() {
    let self = this;

    $(function () {
      /* $(".draggable").draggable({
        helper: "clone",
        start: function() {
          self.dragging_element_name = $(this).attr("id");
        },
      }); */

      $(".draggable").draggable({
        connectToSortable: "#draggable-items-container, .droppable-flex-container",
        helper: "clone",
        cancel: false,
        //revert: "invalid",
        start: function () {
          self.dragging_element_name = $(this).attr("id");
        },
      });

      $("#draggable-items-container").sortable({
        placeholder: "container-frame", //"border-2 border-dashed border-yellow-400 m-2 p-4",
        cancel: false,
        //accept: ".draggable",
        //revert: true,
        /* update: function(event, ui ){
                var item = ui.item;
              var target = ui.item.prev();

              console.log(item, target)
            }, */
        beforeStop: function (event, ui) {

          console.log("beforeStop", event, ui);
          console.log($(ui.item).hasClass("draggable"))

          if (!$(ui.item).hasClass("draggable")) { //self.dragging_element_name === ''
            return
          }

          $(ui.item).replaceWith(self.create_dropped_element());

          self.code_mirror_editor.dispatch({
            changes: {
              from: 0,
              to: self.code_mirror_editor.state.doc.toString().length,
              insert: js_beautify.html($(this).html(), {
                "indent_size": "4",
                "indent_char": " ",
                "max_preserve_newlines": "5",
                "preserve_newlines": true,
              })
            }
          })

          $("#code-editor").text(js_beautify.html($(this).html(),
              {
                "indent_size": "4",
                "indent_char": " ",
                "max_preserve_newlines": "5",
                "preserve_newlines": true,
                "keep_array_indentation": true,
                "break_chained_methods": false,
                "indent_scripts": "keep",
                "brace_style": "none",
                "space_before_conditional": true,
                "unescape_strings": true,
                "jslint_happy": false,
                "end_with_newline": true,
                "wrap_line_length": "0",
                "indent_inner_html": false,
                "comma_first": false,
                "e4x": false,
                "indent_empty_lines": true
              }
          ))
          //self.code_editor.setValue(js_beautify.html( $(this).html(), { indent_size: 2, space_in_empty_parent: true }));
          //self.code_editor.setValue($(this).html());

          self.dragging_element_name = ""
        },
      })
          .click(function (e) {
            $(e.target).find('*').removeClass('selected-element')
            self.selected_element = null
            //$(self.selected_element).removeClass('selected-element')
          })
      /* .droppable({
        drop: function( event, ui ) {
           console.log('drop - ', event, ui)
        }
    }) */

      $(".droppable").droppable({
        accept: ".draggable",
        drop: function (event, ui) {
          console.log("dropped", event, ui);

          let element;
          if (self.dragging_element_name === "input") {
            element = document.createElement("input");
            $(element).addClass("p-2 border rounded-md");
          } else if (self.dragging_element_name === "text") {
            element = document.createElement("p");
            $(element)
                .text("This is text")
                .addClass("text-gray-700");
          } else if (self.dragging_element_name === "button") {
            element = document.createElement("button");
            $(element)
                .text("button")
                .addClass("p-2 border rounded-md");
          } else if (self.dragging_element_name === "flex") {
            element = document.createElement("div");
            $(element)
                .addClass(
                    "flex flex-col gap-4 p-2 border-2 border-dashed border-indigo-500"
                )
                .droppable({
                  accept: ".draggable",
                  classes: {
                    "ui-droppable-hover": "border-dotted",
                  },
                  greedy: true,
                  drop: function (event, ui) {
                    console.log("flex drop");
                    console.log(event);
                    console.log(ui);

                    let child_element;

                    if (self.dragging_element_name === "button") {
                      child_element = document.createElement("button");
                      $(child_element)
                          .text("button")
                          .addClass("p-2 border rounded-md");
                    } else if (self.dragging_element_name === "input") {
                      child_element = document.createElement("input");
                      $(child_element).addClass("p-2 border rounded-md");
                      //.attr('disabled','disabled')
                    } else if (self.dragging_element_name === "text") {
                      child_element = document.createElement("p");
                      $(child_element)
                          .text("This is text")
                          .addClass("text-gray-700");
                    }

                    $(this).append(child_element);
                    $(child_element).contextmenu(function (e) {
                      self.selected_element = e.target
                      console.log("right clicked", $(e.target).attr("class"));
                      return false;
                    });

                    //self.code_editor.setValue(js_beautify.html( $("#main-canvas").html(), { indent_size: 2, space_in_empty_parent: true }))
                  },
                })
                .sortable({
                  cancel: false,
                  placeholder: "bg-yellow-200 p-2",
                  //connectWith: ".connectedSortable"
                });
            //.resizable();
          } else if (self.dragging_element_name === "grid") {
            element = document.createElement("div");
            $(element)
                .addClass("grid grid-cols-3 gap-4 p-2 border")
                .droppable({
                  drop: function () {
                    console.log("grid drop");
                  },
                });
          }

          $(this).append(element);
          $(element).contextmenu(function () {
            /* self.selected_element = e.target
            console.log("right clicked", $(e.target).attr("class"));
            $("#element").text( $(e.target).prop('tagName') )
            */
            return false;
          });

          $(element).click(function (e) {
            self.selected_element = e.target
            console.log("left clicked", $(e.target).attr("class"));
            $("#element")
                .text($(e.target).prop("tagName"))
                .removeClass("hidden");
            return false;
          });

          $("#code-editor").text(
              js_beautify.html($(this).html(), {
                indent_size: 2,
                space_in_empty_parent: true,
              })
          );
          //self.code_editor.setValue(js_beautify.html( $(this).html(), { indent_size: 2, space_in_empty_parent: true }));
          //self.code_editor.setValue($(this).html());
        },
      });

      $(".droppable").sortable({
        cancel: false,
        placeholder: "bg-yellow-200 p-2",
      });
      //$('.droppable').disableSelection();

      //let code_editor = $("#code_editor")
      self.code_editor = CodeMirror(document.getElementById("code_editor"), {
        value: "",
        //mode: "htmlmixed",
        lineNumbers: true,
        //theme: 'monokai',
        //styleActiveLine: true,
        //matchBrackets: true,
        scrollbarStyle: "simple"
      });

      self.code_editor.on("change", function (code_mirror_element) {
        console.log(js_beautify, code_mirror_element.getValue());
        $("#main-canvas").html(code_mirror_element.getValue());
      });

      /* var value = "// The bindings defined specifically in the Sublime Text mode\nvar bindings = {\n";
  var map = CodeMirror.keyMap.sublime;
  for (var key in map) {
    var val = map[key];
    if (key != "fallthrough" && val != "..." && (!/find/.test(val) || /findUnder/.test(val)))
      value += "  \"" + key + "\": \"" + val + "\",\n";
  }
  value += "}\n\n// The implementation of joinLines\n";
  value += CodeMirror.commands.joinLines.toString().replace(/^function\s*\(/, "function joinLines(").replace(/\n  /g, "\n") + "\n"; */
      /* CodeMirror(document.getElementById("code_editor"), {
    value: 'value',
    lineNumbers: true,
    mode: "javascript",
    keyMap: "sublime",
    autoCloseBrackets: true,
    matchBrackets: true,
    showCursorWhenSelecting: true,
    theme: "monokai",
    tabSize: 2
  }); */

      $('#code-editor').on('input', function () {
        $('#draggable-items-container').html($(this).text())
      })

      self.code_mirror_editor = new EditorView({
        state: EditorState.create({
          extensions: [
            basicSetup,
            html(),
            EditorView.updateListener.of((v) => {
              if (v.docChanged) {
                //console.log(v.state.doc.toString());
                //$('#draggable-items-container').html(v.state.doc.toString())
              }
            })
          ],
          doc: ''
        }),
        parent: document.getElementById('code-mirror-editor')
      })

      self.codemirror_classes_editor = new EditorView({
        state: EditorState.create({
          extensions: [
            basicSetup,
            html(),
            EditorView.updateListener.of((v) => {
              if (v.docChanged) {
                console.log(v.state.doc.toString());
                $(self.selected_element).attr("class", v.state.doc.toString())
              }
            }),
          ],
          doc: ''
        }),
        parent: document.getElementById('selected_element_classes')
      })

      /*$( '#draggable-items-container' ).tooltip({
        trigger : 'hover',
        classes: {
          "ui-tooltip": "bg-yellow-100 text-xs rounded-md border-none"
        },
        position: {
          my: "center bottom-5",
          at: "center top",
          using: function( position, feedback ) {
            $( this ).css( position );
            $( "<div>" )
                .addClass( "bg-yellow-500" )
                .addClass( feedback.vertical )
                .addClass( feedback.horizontal )
                .appendTo( this );
          }
        }
      });*/

      $("textarea").each(function () {
        //this.setAttribute("style", "height:" + (this.scrollHeight) + "px;overflow-y:hidden;");
      }).on("input click", function () {
        //this.style.height = "auto";
        //this.style.height = (this.scrollHeight) + "px";
      });

      tippy('.properties', {
        //content: 'Drop elements here',
        trigger: 'click',
        content(reference) {
          const id = reference.getAttribute('data-template');
          const template = document.getElementById(id);
          //return template.innerHTML;

          let element = document.createElement("div");
          $(element).html(template).click(function(tippy_style_elem){
            let data_class_attr = $(tippy_style_elem.target).attr('data-class')
            if(data_class_attr !== null){
              reference._tippy.hide()

              $(self.selected_element).addClass(data_class_attr).addClass('flex')
              self.selected_element_classes = $(self.selected_element).attr('class')
            }
          })
          return element
        },
        theme: 'light-border',
        allowHTML: true,
        arrow: false,
        interactive: true,
        placement: 'left',
        /*onCreate(instance) {
          // Setup our own custom state properties
          /!*instance._isFetching = false;
          instance._src = null;
          instance._error = null;*!/
          console.log(instance)
        },*/
        onShow(instance) {

          //instance.setContent("onShow")
          //console.log(instance.reference.id)

          /*if (instance._isFetching || instance._src || instance._error) {
            return;
          }*/

          /*let element
          element = document.createElement("div");
          $(element).addClass("m-2 p-2 border rounded-md").text(instance.reference.id)
          .click(function(){
            console.log('click')
          })*/

          //instance.setContent(element)

          $(instance).click(function () {
            alert('click')
          })
        }
      });

      $(document).click(function (e) {
        console.log('docs click', e.target)
      });
    });
  },
  watch: {
    selected_element() {
      if ($(this.selected_element).prop("tagName") === 'INPUT') {
        this.selected_element_inner_html = $(this.selected_element).val()
      } else if ($(this.selected_element).prop("tagName") === 'IMG') {
        this.selected_element_inner_html = $(this.selected_element).attr('src')
      } else {
        this.selected_element_inner_html = $(this.selected_element).html()
      }

      // filling up classes editor
      this.codemirror_classes_editor.dispatch({
        changes: {
          from: 0,
          to: this.codemirror_classes_editor.state.doc.toString().length,
          insert: $(this.selected_element).attr('class')
        }
      })
    },
    selected_element_inner_html() {
      if ($(this.selected_element).prop("tagName") === 'INPUT') {
        $(this.selected_element).val(this.selected_element_inner_html)
      } else if ($(this.selected_element).prop("tagName") === 'IMG') {
        $(this.selected_element).attr('src', this.selected_element_inner_html)
      } else {
        $(this.selected_element).html(this.selected_element_inner_html)
      }
    },
    selected_element_classes() {
      $(this.selected_element).attr("class", this.selected_element_classes)
    }
    /*selected_element_html(){
      let self = this
      console.log($(this.selected_element))
      setTimeout(function (){
        console.log(self.selected_element_html)

        //$(self.selected_element).replaceWith(self.selected_element_html)
      }, 3000)
    }*/
  },
};
</script>
