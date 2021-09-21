<template>
  <div class="flex items-center p-4 gap-4">
    <h1 class="lg:text-2xl font-medium text-gray-500">UI Builder for</h1>
    <img
        src="https://tailwindcss.com/_next/static/media/tailwindcss-logotype.128b6e12eb85d013bc9f80a917f57efe.svg"
        alt="TailwindCSS"
        class="h-6 w-auto"
    />
    <button class="inline-flex text-sm items-center border p-2 rounded-md font-medium hover:bg-gray-200"
            @click="clear_canvas" title="Clear Canvas">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-red-500 mr-1" fill="none" viewBox="0 0 24 24"
           stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
              d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16"/>
      </svg>
      Clear canvas
    </button>
    <button
        class="inline-flex text-sm items-center p-2 rounded-md font-medium bg-indigo-600 hover:bg-indigo-400 text-white"
        @click="clear_canvas" title="Clear Canvas">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" fill="none" viewBox="0 0 24 24"
           stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
              d="M10 20l4-16m4 4l4 4-4 4M6 16l-4-4 4-4"/>
      </svg>
      Get code
    </button>
    <!-- <button @click="clear_canvas">Preview</button>-->
  </div>
  <div class="grid grid-cols-8 gap-4">
    <div class="flex flex-col col-span-1" title="Drag Elements to canvas">
      <!-- <div class="draggable cursor-move select-none">Container</div> -->
      <div class="p-2 draggable cursor-move select-none inline-flex items-center text-sm text-gray-500" id="text">
        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-textarea-t"
             viewBox="0 0 16 16">
          <path
              d="M1.5 2.5A1.5 1.5 0 0 1 3 1h10a1.5 1.5 0 0 1 1.5 1.5v3.563a2 2 0 0 1 0 3.874V13.5A1.5 1.5 0 0 1 13 15H3a1.5 1.5 0 0 1-1.5-1.5V9.937a2 2 0 0 1 0-3.874V2.5zm1 3.563a2 2 0 0 1 0 3.874V13.5a.5.5 0 0 0 .5.5h10a.5.5 0 0 0 .5-.5V9.937a2 2 0 0 1 0-3.874V2.5A.5.5 0 0 0 13 2H3a.5.5 0 0 0-.5.5v3.563zM2 7a1 1 0 1 0 0 2 1 1 0 0 0 0-2zm12 0a1 1 0 1 0 0 2 1 1 0 0 0 0-2z"/>
          <path
              d="M11.434 4H4.566L4.5 5.994h.386c.21-1.252.612-1.446 2.173-1.495l.343-.011v6.343c0 .537-.116.665-1.049.748V12h3.294v-.421c-.938-.083-1.054-.21-1.054-.748V4.488l.348.01c1.56.05 1.963.244 2.173 1.496h.386L11.434 4z"/>
        </svg>
        <span class="ml-2">TEXT</span>
      </div>
      <div class="p-2 draggable cursor-move select-none inline-flex items-center text-sm text-gray-500" id="button">
        <button class="bg-black text-white text-xs p-2 rounded-md">Button</button>
      </div>
      <div class="p-2 draggable cursor-move select-none" id="input">
        <input placeholder="Write here" type="text"
               class="w-full text-xs border border-indigo-600 outline-none rounded p-2">
      </div>
      <!-- <div class="draggable cursor-move select-none" id="grid">Grid</div> -->
      <div class="p-2 draggable cursor-move select-none" id="flex">
        <div class="w-full p-2 inline-flex space-x-2 bg-indigo-100 bg-stripes bg-stripes-white rounded-md">
          <div class="rounded-md text-white font-extrabold text-center bg-indigo-400 h-6 w-6"></div>
          <div class="rounded-md text-white font-extrabold text-center bg-indigo-400 h-6 w-6"></div>
          <div class="rounded-md text-white font-extrabold text-center bg-indigo-400 h-6 w-6"></div>
        </div>
      </div>
      <div class="p-1 draggable cursor-move select-none" id="img">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-10 w-10" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z"/>
        </svg>
      </div>
    </div>
    <div
        class="border border-dashed col-span-5"
        id="draggable-items-container"
    ></div>
    <div
        id="main-canvas"
        class="droppable min-h-full w-full border hidden"
    ></div>

    <div class="flex flex-col space-y-2 col-span-2">
      <button class="border border-green-400" @click="duplicate_element">Duplicate element</button>
      <button class="bg-red-500 text-white" @click="delete_element">Delete element</button>
      <button class="bg-gray-400 text-black" @click="delete_element">Remove border & ring</button>
      <div id="element" class="hidden"></div>
      <textarea v-model="selected_element_classes"
                class="resize-none border border-green-500 outline-none p-2 rounded-md mx-2"
                placeholder="Classes"></textarea>
      <textarea v-model="selected_element_inner_html"
                class="resize-none border border-green-500 outline-none p-2 rounded-md mx-2"
                placeholder="HTML / Value"></textarea>
      <textarea class="border border-black hidden" v-model="selected_element_html" @keyup="element_html_change"
                id="element-html"></textarea>

      <div>
        Flex
        <select @change="add_class" id="flex-row-or-col">
          <option>flex-row</option>
          <option>flex-col</option>
        </select>
      </div>

      <div>
        Attributes
      </div>
      <div class="grid grid-cols-4 gap-2">
        <p class="col-span-4">Margin</p>
        <select class="appearance-none" @change="add_class" id="m">
          <option>m-0</option>
          <option>m-1</option>
          <option>m-2</option>
          <option>m-3</option>
        </select>
        <select @change="add_class" id="m">
          <option>m-0</option>
          <option>m-1</option>
          <option>m-2</option>
          <option>m-3</option>
        </select>
        <select @change="add_class" id="m">
          <option>m-0</option>
          <option>m-1</option>
          <option>m-2</option>
          <option>m-3</option>
        </select>
        <select @change="add_class" id="m">
          <option>m-0</option>
          <option>m-1</option>
          <option>m-2</option>
          <option>m-3</option>
        </select>
      </div>
      <div>Padding</div>
      <select @change="add_class" id="p">
        <option>p-0</option>
        <option value="p-1">p-1</option>
        <option>p-2</option>
        <option>p-3</option>
      </select>
      <div>
        Border
        <select @change="add_class" id="border-color">
          <option v-for="border_color in border_colors" :key="border_color">{{ border_color }}</option>
        </select>
      </div>
      <div>
        Ring
        <select @change="add_class" id="ring-color">
          <option v-for="ring_color in ring_colors" :key="ring_color">{{ ring_color }}</option>
        </select>
      </div>
      <div>Width - Height</div>
      <select @change="add_class" id="w">
        <option v-for="w in widths" :key="w">{{ w }}</option>
      </select>
      <div>Alignment</div>
      <div>Background color
        <select @change="add_class" id="bg">
          <option v-for="bg_color in bg_colors" :key="bg_color">{{ bg_color }}</option>
        </select>
      </div>
      <div>
        Text color
        <select @change="add_class" id="text-color">
          <option v-for="text_color in text_colors" :key="text_color">{{ text_color }}</option>
        </select>
      </div>
      <div>Text size</div>
      <div>Shadow</div>
      <div>
        Radius
        <select @change="add_class" id="border-radiuses">
          <option v-for="border_radius in border_radiuses" :key="border_radius">{{ border_radius }}</option>
        </select>
      </div>
      <div>Position</div>
      <div>
        Float
      </div>
    </div>
  </div>
  <div id="code-mirror-editor"></div>
  <pre
      contenteditable
      class="hidden overflow-x-auto mx-auto my-8 outline-none max-w-4xl border resize font-mono text-sm p-2"
      id="code-editor"
  ></pre>
  <div class="border rounded-md m-6 hidden" id="code_editor"></div>
  <div id="template" class="hidden">
    <strong>Bolded content</strong>
  </div>

  <div>
    <button
        type="button"
        @click="openModal"
        class="px-4 py-2 text-sm font-medium text-white bg-black rounded-md bg-opacity-20 hover:bg-opacity-30 focus:outline-none focus-visible:ring-2 focus-visible:ring-white focus-visible:ring-opacity-75"
    >
      Open dialog
    </button>
  </div>
  <TransitionRoot appear :show="isOpen" as="template">
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
            <DialogOverlay class="fixed inset-0" />
          </TransitionChild>

          <span class="inline-block h-screen align-middle" aria-hidden="true">
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
              <DialogTitle
                  as="h3"
                  class="text-lg font-medium leading-6 text-gray-900"
              >
                Payment successful
              </DialogTitle>
              <div class="mt-2">
                <p class="text-sm text-gray-500">
                  Your payment has been successfully submitted. We’ve sent you
                  an email with all of the details of your order.
                </p>
              </div>

              <div class="mt-4">
                <button
                    type="button"
                    class="inline-flex justify-center px-4 py-2 text-sm font-medium text-blue-900 bg-blue-100 border border-transparent rounded-md hover:bg-blue-200 focus:outline-none focus-visible:ring-2 focus-visible:ring-offset-2 focus-visible:ring-blue-500"
                    @click="closeModal"
                >
                  Got it, thanks!
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
//import tippy from 'tippy.js';
import 'tippy.js/dist/tippy.css';

import { ref } from "vue";
import {
  Dialog,
  DialogOverlay,
  DialogTitle,
  TransitionRoot,
  TransitionChild,
} from "@headlessui/vue";

export default {

  components: { Dialog, DialogOverlay, DialogTitle, TransitionRoot, TransitionChild },
  setup() {
    const isOpen = ref(false)

    return {
      isOpen,

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
      code_editor: null,
      selected_element: null,
      selected_element_html: "",
      selected_element_inner_html: "",
      selected_element_classes: "",
      
      margins: [
        "m-0",
"m-px",
"m-0.5",
"m-1",
"m-1.5",
"m-2",
"m-2.5",
"m-3",
"m-3.5",
"m-4",
"m-5",
"m-6",
"m-7",
"m-8",
"m-9",
"m-10",
"m-11",
"m-12",
"m-14",
"m-16",
"m-20",
"m-24",
"m-28",
"m-32",
"m-36",
"m-40",
"m-44",
"m-48",
"m-52",
"m-56",
"m-60",
"m-64",
"m-72",
"m-80",
"m-96",
"m-auto",
"-m-0",
"-m-px",
"-m-0.5",
"-m-1",
"-m-1.5",
"-m-2",
"-m-2.5",
"-m-3",
"-m-3.5",
"-m-4",
"-m-5",
"-m-6",
"-m-7",
"-m-8",
"-m-9",
"-m-10",
"-m-11",
"-m-12",
"-m-14",
"-m-16",
"-m-20",
"-m-24",
"-m-28",
"-m-32",
"-m-36",
"-m-40",
"-m-44",
"-m-48",
"-m-52",
"-m-56",
"-m-60",
"-m-64",
"-m-72",
"-m-80",
"-m-96",
"mx-0",
"mx-px",
"mx-0.5",
"mx-1",
"mx-1.5",
"mx-2",
"mx-2.5",
"mx-3",
"mx-3.5",
"mx-4",
"mx-5",
"mx-6",
"mx-7",
"mx-8",
"mx-9",
"mx-10",
"mx-11",
"mx-12",
"mx-14",
"mx-16",
"mx-20",
"mx-24",
"mx-28",
"mx-32",
"mx-36",
"mx-40",
"mx-44",
"mx-48",
"mx-52",
"mx-56",
"mx-60",
"mx-64",
"mx-72",
"mx-80",
"mx-96",
"mx-auto",
"-mx-0",
"-mx-px",
"-mx-0.5",
"-mx-1",
"-mx-1.5",
"-mx-2",
"-mx-2.5",
"-mx-3",
"-mx-3.5",
"-mx-4",
"-mx-5",
"-mx-6",
"-mx-7",
"-mx-8",
"-mx-9",
"-mx-10",
"-mx-11",
"-mx-12",
"-mx-14",
"-mx-16",
"-mx-20",
"-mx-24",
"-mx-28",
"-mx-32",
"-mx-36",
"-mx-40",
"-mx-44",
"-mx-48",
"-mx-52",
"-mx-56",
"-mx-60",
"-mx-64",
"-mx-72",
"-mx-80",
"-mx-96",
"my-0",
"my-px",
"my-0.5",
"my-1",
"my-1.5",
"my-2",
"my-2.5",
"my-3",
"my-3.5",
"my-4",
"my-5",
"my-6",
"my-7",
"my-8",
"my-9",
"my-10",
"my-11",
"my-12",
"my-14",
"my-16",
"my-20",
"my-24",
"my-28",
"my-32",
"my-36",
"my-40",
"my-44",
"my-48",
"my-52",
"my-56",
"my-60",
"my-64",
"my-72",
"my-80",
"my-96",
"my-auto",
"-my-0",
"-my-px",
"-my-0.5",
"-my-1",
"-my-1.5",
"-my-2",
"-my-2.5",
"-my-3",
"-my-3.5",
"-my-4",
"-my-5",
"-my-6",
"-my-7",
"-my-8",
"-my-9",
"-my-10",
"-my-11",
"-my-12",
"-my-14",
"-my-16",
"-my-20",
"-my-24",
"-my-28",
"-my-32",
"-my-36",
"-my-40",
"-my-44",
"-my-48",
"-my-52",
"-my-56",
"-my-60",
"-my-64",
"-my-72",
"-my-80",
"-my-96",
"mt-0",
"mt-px",
"mt-0.5",
"mt-1",
"mt-1.5",
"mt-2",
"mt-2.5",
"mt-3",
"mt-3.5",
"mt-4",
"mt-5",
"mt-6",
"mt-7",
"mt-8",
"mt-9",
"mt-10",
"mt-11",
"mt-12",
"mt-14",
"mt-16",
"mt-20",
"mt-24",
"mt-28",
"mt-32",
"mt-36",
"mt-40",
"mt-44",
"mt-48",
"mt-52",
"mt-56",
"mt-60",
"mt-64",
"mt-72",
"mt-80",
"mt-96",
"mt-auto",
"-mt-0",
"-mt-px",
"-mt-0.5",
"-mt-1",
"-mt-1.5",
"-mt-2",
"-mt-2.5",
"-mt-3",
"-mt-3.5",
"-mt-4",
"-mt-5",
"-mt-6",
"-mt-7",
"-mt-8",
"-mt-9",
"-mt-10",
"-mt-11",
"-mt-12",
"-mt-14",
"-mt-16",
"-mt-20",
"-mt-24",
"-mt-28",
"-mt-32",
"-mt-36",
"-mt-40",
"-mt-44",
"-mt-48",
"-mt-52",
"-mt-56",
"-mt-60",
"-mt-64",
"-mt-72",
"-mt-80",
"-mt-96",
"mr-0",
"mr-px",
"mr-0.5",
"mr-1",
"mr-1.5",
"mr-2",
"mr-2.5",
"mr-3",
"mr-3.5",
"mr-4",
"mr-5",
"mr-6",
"mr-7",
"mr-8",
"mr-9",
"mr-10",
"mr-11",
"mr-12",
"mr-14",
"mr-16",
"mr-20",
"mr-24",
"mr-28",
"mr-32",
"mr-36",
"mr-40",
"mr-44",
"mr-48",
"mr-52",
"mr-56",
"mr-60",
"mr-64",
"mr-72",
"mr-80",
"mr-96",
"mr-auto",
"-mr-0",
"-mr-px",
"-mr-0.5",
"-mr-1",
"-mr-1.5",
"-mr-2",
"-mr-2.5",
"-mr-3",
"-mr-3.5",
"-mr-4",
"-mr-5",
"-mr-6",
"-mr-7",
"-mr-8",
"-mr-9",
"-mr-10",
"-mr-11",
"-mr-12",
"-mr-14",
"-mr-16",
"-mr-20",
"-mr-24",
"-mr-28",
"-mr-32",
"-mr-36",
"-mr-40",
"-mr-44",
"-mr-48",
"-mr-52",
"-mr-56",
"-mr-60",
"-mr-64",
"-mr-72",
"-mr-80",
"-mr-96",
"mb-0",
"mb-px",
"mb-0.5",
"mb-1",
"mb-1.5",
"mb-2",
"mb-2.5",
"mb-3",
"mb-3.5",
"mb-4",
"mb-5",
"mb-6",
"mb-7",
"mb-8",
"mb-9",
"mb-10",
"mb-11",
"mb-12",
"mb-14",
"mb-16",
"mb-20",
"mb-24",
"mb-28",
"mb-32",
"mb-36",
"mb-40",
"mb-44",
"mb-48",
"mb-52",
"mb-56",
"mb-60",
"mb-64",
"mb-72",
"mb-80",
"mb-96",
"mb-auto",
"-mb-0",
"-mb-px",
"-mb-0.5",
"-mb-1",
"-mb-1.5",
"-mb-2",
"-mb-2.5",
"-mb-3",
"-mb-3.5",
"-mb-4",
"-mb-5",
"-mb-6",
"-mb-7",
"-mb-8",
"-mb-9",
"-mb-10",
"-mb-11",
"-mb-12",
"-mb-14",
"-mb-16",
"-mb-20",
"-mb-24",
"-mb-28",
"-mb-32",
"-mb-36",
"-mb-40",
"-mb-44",
"-mb-48",
"-mb-52",
"-mb-56",
"-mb-60",
"-mb-64",
"-mb-72",
"-mb-80",
"-mb-96",
"ml-0",
"ml-px",
"ml-0.5",
"ml-1",
"ml-1.5",
"ml-2",
"ml-2.5",
"ml-3",
"ml-3.5",
"ml-4",
"ml-5",
"ml-6",
"ml-7",
"ml-8",
"ml-9",
"ml-10",
"ml-11",
"ml-12",
"ml-14",
"ml-16",
"ml-20",
"ml-24",
"ml-28",
"ml-32",
"ml-36",
"ml-40",
"ml-44",
"ml-48",
"ml-52",
"ml-56",
"ml-60",
"ml-64",
"ml-72",
"ml-80",
"ml-96",
"ml-auto",
"-ml-0",
"-ml-px",
"-ml-0.5",
"-ml-1",
"-ml-1.5",
"-ml-2",
"-ml-2.5",
"-ml-3",
"-ml-3.5",
"-ml-4",
"-ml-5",
"-ml-6",
"-ml-7",
"-ml-8",
"-ml-9",
"-ml-10",
"-ml-11",
"-ml-12",
"-ml-14",
"-ml-16",
"-ml-20",
"-ml-24",
"-ml-28",
"-ml-32",
"-ml-36",
"-ml-40",
"-ml-44",
"-ml-48",
"-ml-52",
"-ml-56",
"-ml-60",
"-ml-64",
"-ml-72",
"-ml-80",
"-ml-96",
      ],
      widths: ["w-0", "w-px", "w-0.5", "w-1", "w-1.5", "w-2", "w-2.5", "w-3", "w-3.5", "w-4",
        "w-5", "w-6", "w-7", "w-8", "w-9", "w-10", "w-11", "w-12", "w-14", "w-16", "w-20",
        "w-24", "w-28", "w-32", "w-36", "w-40", "w-44", "w-48", "w-52", "w-56", "w-60", "w-64",
        "w-72", "w-80", "w-96", "w-auto", "w-1/2", "w-1/3", "w-2/3", "w-1/4", "w-2/4", "w-3/4",
        "w-1/5", "w-2/5", "w-3/5", "w-4/5", "w-1/6", "w-2/6", "w-3/6", "w-4/6", "w-5/6", "w-1/12",
        "w-2/12", "w-3/12", "w-4/12", "w-5/12", "w-6/12", "w-7/12", "w-8/12", "w-9/12", "w-10/12",
        "w-11/12", "w-full", "w-screen", "w-min", "w-max"
      ],
      bg_colors: [
        "bg-transparent", "bg-current", "bg-black", "bg-white",
        "bg-gray-50", "bg-red-50", "bg-yellow-50", "bg-green-50",
        "bg-blue-50", "bg-indigo-50", "bg-purple-50", "bg-pink-50",
      ],
      text_colors: [
        "text-transparent", "text-current", "text-black", "text-white",
        "text-gray-500", "text-red-500", "text-yellow-500", "text-green-500",
        "text-blue-500", "text-indigo-500", "text-purple-500", "text-pink-500",
      ],
      border_colors: [
        "border-transparent", "border-current", "border-black", "border-white",
        "border-gray-500", "border-red-500", "border-yellow-500", "border-green-500",
        "border-blue-500", "border-indigo-500", "border-purple-500", "border-pink-500",
      ],
      border_radiuses: [
        "rounded-none", "rounded-sm", "rounded", "rounded-md", "rounded-lg",
        "rounded-xl", "rounded-2xl", "rounded-3xl", "rounded-full", "rounded-t-none",
        "rounded-t-sm", "rounded-t", "rounded-t-md", "rounded-t-lg", "rounded-t-xl",
        "rounded-t-2xl", "rounded-t-3xl", "rounded-t-full", "rounded-r-none",
        "rounded-r-sm", "rounded-r", "rounded-r-md",
        "rounded-r-lg",	"rounded-r-xl", "rounded-r-2xl", "rounded-r-3xl", "rounded-r-full",
        "rounded-b-none",	"rounded-b-sm", "rounded-b", "rounded-b-md", "rounded-b-lg",
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
        "ring-gray-500", "ring-red-500", "ring-yellow-500", "ring-green-500",
        "ring-blue-500", "ring-indigo-500", "ring-purple-500", "ring-pink-500",
      ]
    };
  },
  methods: {
    add_class(event) {
      if (this.selected_element === null) {
        alert("To apply designs select one element by right clicking.");
        return;
      }

      let class_to_add = $(event.currentTarget).val();
      let classes = $(this.selected_element).attr("class"); //.split(/\s+/)

      let style_to_apply = $(event.currentTarget).attr("id");

      if (style_to_apply === "p") {
        classes = classes.replace(/(^|\s)p-\d+/gim, class_to_add);

        if (classes.indexOf(class_to_add) === -1) {
          classes += " " + class_to_add + " ";
        }
      } else if (style_to_apply === "m") {
        classes = classes.replace(/m-\d+/gim, class_to_add);

        if (classes.indexOf(class_to_add) === -1) {
          classes += " " + class_to_add + " ";
        }
      } else if (style_to_apply === "bg") {
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
        classes = classes.replace(/(^|\s)border-\w+-\d+/gim, " " + class_to_add + " ");

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
      }

      console.log(class_to_add, classes);
      $(this.selected_element).attr("class", classes);

      this.selected_element_html = $(this.selected_element).prop("outerHTML")
      this.selected_element_classes = $(this.selected_element).attr("class")
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
    create_dropped_element() {
      let self = this

      let element;
      if (self.dragging_element_name === "input") {
        element = document.createElement("input");
        $(element).addClass("m-2 p-2 border rounded-md");
      } else if (self.dragging_element_name === "text") {
        element = document.createElement("p");
        $(element)
            .text("You have just dropped a text element. Edit text by selecting this element.")
            .addClass("p-2 m-2 text-gray-600");
      } else if (self.dragging_element_name === "button") {
        element = document.createElement("button");
        $(element)
            .text("Button")
            .addClass("m-2 bg-black text-white py-2 px-4 rounded-md")
        //.attr('title', 'BUTTON Element')
      } else if (self.dragging_element_name === "img") {
        element = document.createElement("img");
        $(element)
            .attr('src', "https://tailwindcss.com/_next/static/media/tailwindcss-logotype.128b6e12eb85d013bc9f80a917f57efe.svg")
            .addClass("m-2");
      } else if (self.dragging_element_name === "flex") {
        element = document.createElement("div");
        $(element)
            .addClass(
                "border-2 border-dashed border-green-200 m-2 p-2 flex flex-col gap-4 droppable-flex-container"
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
              placeholder: "border-2 border-dashed border-red-400 m-2 p-4",
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
        //alert('Apply style')
        /* self.selected_element = $(e.target)
        console.log("right clicked", $(e.target).attr("class"));
        $("#element").text( $(e.target).prop('tagName') )
        */
        self.isOpen = true
        return false;
      });

      $(element).click(function (e) {
        self.selected_element = e.target
        console.log("left clicked", $(e.target).attr("class"));
        //$("#element").text( $(e.target).prop('tagName') ).removeClass("hidden")
        //$("#element-html").val($(e.target).prop('outerHTML'))
        self.selected_element_html = $(e.target).prop('outerHTML')

        $("#draggable-items-container").find('*')/*.children()*/.removeClass('selected-element')
        $(e.target).addClass('selected-element')
        self.selected_element_classes = $(e.target).attr('class')
        return false;
      });

      /*tippy(element, {
        //content: $(element).prop('outerHTML'),
        content: $('#template').html(),
        interactive: true,
        trigger: 'click',
        allowHTML: true,
        //hideOnClick: 'toggle',
      });*/

      return element
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
        placeholder: "border-2 border-dashed border-yellow-400 m-2 p-4",
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
        this.setAttribute("style", "height:" + (this.scrollHeight) + "px;overflow-y:hidden;");
      }).on("input click", function () {
        this.style.height = "auto";
        this.style.height = (this.scrollHeight) + "px";
      });

      /*tippy('#draggable-items-container', {
        content: 'Drop elements here',
      });*/

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
