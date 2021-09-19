<template>
  <div class="flex items-center p-4 gap-4">
    <h1 class="text-2xl font-medium text-gray-500">UI Builder for</h1>
    <img
        src="https://tailwindcss.com/_next/static/media/tailwindcss-logotype.128b6e12eb85d013bc9f80a917f57efe.svg"
        alt="TailwindCSS"
        class="h-6 w-auto"
    />
    <button class="inline-flex text-sm items-center border p-2 rounded-md font-medium hover:bg-gray-200" @click="clear_canvas" title="Clear Canvas">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-red-500 mr-1" fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
      </svg>
      Clear canvas
    </button>
    <button class="inline-flex text-sm items-center p-2 rounded-md font-medium bg-indigo-600 hover:bg-indigo-400 text-white" @click="clear_canvas" title="Clear Canvas">
      <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 mr-2" fill="none" viewBox="0 0 24 24" stroke="currentColor">
        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 20l4-16m4 4l4 4-4 4M6 16l-4-4 4-4" />
      </svg>
      Get code
    </button>
    <!-- <button @click="clear_canvas">Preview</button>-->
  </div>
  <div class="grid grid-cols-8">
    <div class="flex flex-col col-span-1" title="Drag Elements to canvas">
      <!-- <div class="draggable cursor-move select-none">Container</div> -->
      <div class="p-2 draggable cursor-move select-none inline-flex items-center text-sm text-gray-500" id="text">
        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-textarea-t" viewBox="0 0 16 16">
          <path d="M1.5 2.5A1.5 1.5 0 0 1 3 1h10a1.5 1.5 0 0 1 1.5 1.5v3.563a2 2 0 0 1 0 3.874V13.5A1.5 1.5 0 0 1 13 15H3a1.5 1.5 0 0 1-1.5-1.5V9.937a2 2 0 0 1 0-3.874V2.5zm1 3.563a2 2 0 0 1 0 3.874V13.5a.5.5 0 0 0 .5.5h10a.5.5 0 0 0 .5-.5V9.937a2 2 0 0 1 0-3.874V2.5A.5.5 0 0 0 13 2H3a.5.5 0 0 0-.5.5v3.563zM2 7a1 1 0 1 0 0 2 1 1 0 0 0 0-2zm12 0a1 1 0 1 0 0 2 1 1 0 0 0 0-2z"/>
          <path d="M11.434 4H4.566L4.5 5.994h.386c.21-1.252.612-1.446 2.173-1.495l.343-.011v6.343c0 .537-.116.665-1.049.748V12h3.294v-.421c-.938-.083-1.054-.21-1.054-.748V4.488l.348.01c1.56.05 1.963.244 2.173 1.496h.386L11.434 4z"/>
        </svg>
        <span class="ml-2">TEXT</span>
      </div>
      <div class="p-2 draggable cursor-move select-none inline-flex items-center text-sm text-gray-500" id="button">
        <button class="bg-black text-white text-xs p-2 rounded-md">Button</button>
      </div>
      <div class="p-2 draggable cursor-move select-none" id="input">
        <input placeholder="Write here" type="text" class="w-3/4 text-xs border border-indigo-600 outline-none rounded p-2">
      </div>
      <!-- <div class="draggable cursor-move select-none" id="grid">Grid</div> -->
      <div class="p-2 draggable cursor-move select-none" id="flex">
        <div class="p-2 inline-flex space-x-2 bg-indigo-100 bg-stripes bg-stripes-white rounded-md">
          <div class="flex-1 rounded-md text-white font-extrabold text-center bg-indigo-400 px-3 py-1">1</div>
          <div class="flex-1 rounded-md text-white font-extrabold text-center bg-indigo-400 px-3 py-1">2</div>
          <div class="flex-1 rounded-md text-white font-extrabold text-center bg-indigo-400 px-3 py-1">3</div></div>
      </div>
      <div class="p-1 draggable cursor-move select-none" id="img">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-12 w-12" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z" />
        </svg>
      </div>
    </div>
    <div class="col-span-5">
      <div
          class="p-6 border border-dashed"
          id="draggable-items-container"
      ></div>
      <div
          id="main-canvas"
          class="droppable min-h-full w-full border hidden"
      ></div>
    </div>
    <div class="flex flex-col col-span-2">
      <div id="element" class="hidden"></div>
      <input v-model="selected_element_classes" class="border border-green-500 outline-none p-2" type="text"
             placeholder="Classes">
      <input v-model="selected_element_inner_html" class="border border-green-500 outline-none p-2" type="text"
             placeholder="innerHTML / value">
      <textarea class="border border-black hidden" v-model="selected_element_html" @keyup="element_html_change"
                id="element-html"></textarea>
      <div>Margin</div>
      <select @change="add_class" id="m">
        <option>m-0</option>
        <option>m-1</option>
        <option>m-24</option>
        <option>m-3</option>
      </select>
      <div>Padding</div>
      <select @change="add_class" id="p">
        <option>p-0</option>
        <option value="p-1">p-1</option>
        <option>p-2</option>
        <option>p-3</option>
      </select>
      <div>Border</div>
      <div>Width - Height</div>
      <div>Alignment</div>
      <div>Background color
        <select @change="add_class" id="bg">
          <option value="" selected>Not selected</option>
          <option>bg-green-300</option>
          <option>bg-indigo-300</option>
          <option>bg-red-400</option>
        </select>
      </div>
      <div>Text color</div>
      <div>Text size</div>
      <div>Shadow</div>
      <div>Radius</div>
      <div>Position</div>
      <div>
        Float
      </div>
      <div>
        Flex

        <div class="p-4 inline-flex space-x-4 bg-indigo-50 bg-stripes bg-stripes-white rounded-md">
          <div class="flex-1 rounded-md text-white font-extrabold text-center bg-indigo-500 px-6 py-4">1</div>
          <div class="flex-1 rounded-md text-white font-extrabold text-center bg-indigo-500 px-6 py-4">2</div>
          <div class="flex-1 rounded-md text-white font-extrabold text-center bg-indigo-500 px-6 py-4">3</div>
        </div>

        <select>
          <option>flex-row</option>
          <option>flex-col</option>
        </select>
      </div>
      <button class="border border-green-400" @click="duplicate_element">Duplicate element</button>
      <button class="bg-red-500 text-white" @click="delete_element">Delete element</button>
    </div>
  </div>
  <div id="code-mirror-editor"></div>
  <pre
      contenteditable
      class="hidden overflow-x-auto mx-auto my-8 outline-none max-w-4xl border resize font-mono text-sm p-2"
      id="code-editor"
  ></pre>
  <div class="border rounded-md m-6 hidden" id="code_editor"></div>
</template>

<script>
import $ from "jquery";
import "jquery-ui/ui/widgets/draggable";
import "jquery-ui/ui/widgets/droppable";
import "jquery-ui/ui/widgets/sortable";
import "jquery-ui/ui/widgets/resizable";
import "jquery-ui/ui/widgets/tooltip";
import "jquery-ui/themes/base/resizable.css";
import "jquery-ui/themes/base/tooltip.css";
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

export default {
  name: "MainFrame",
  data() {
    return {
      dragging_element_name: "",
      code_mirror_editor: null,
      code_editor: null,
      selected_element: null,
      selected_element_html: "",
      selected_element_inner_html: "",
      selected_element_classes: ""
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
        classes = classes.replace(/p-\d+/gim, class_to_add);

        if (classes.indexOf(class_to_add) === -1) {
          classes += " " + class_to_add + " ";
        }
      } else if (style_to_apply === "m") {
        classes = classes.replace(/m-\d+/gim, class_to_add);

        if (classes.indexOf(class_to_add) === -1) {
          classes += " " + class_to_add + " ";
        }
      } else if (style_to_apply === "bg") {
        classes = classes.replace(/bg-\w+-\d+/gim, class_to_add);

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
        $(element).addClass("p-2 border rounded-md");
      } else if (self.dragging_element_name === "text") {
        element = document.createElement("p");
        $(element)
            .text("You have just dropped a text element. Edit text by selecting this element.")
            .addClass("text-gray-700");
      } else if (self.dragging_element_name === "button") {
        element = document.createElement("button");
        $(element)
            .text("button")
            .addClass("p-2 border rounded-md")
            .attr('title', 'BUTTON Element')
      } else if (self.dragging_element_name === "img") {
        element = document.createElement("img");
        $(element)
            .attr('src', "https://tailwindcss.com/_next/static/media/tailwindcss-logotype.128b6e12eb85d013bc9f80a917f57efe.svg")
            .addClass("m-2");
      } else if (self.dragging_element_name === "flex") {
        element = document.createElement("div");
        $(element)
            .addClass(
                "flex flex-col gap-4 px-2 py-8 border-2 border-dashed border-indigo-500 droppable-flex-container"
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
              placeholder: "bg-red-200 p-2",
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
        alert('Apply style')
        /* self.selected_element = $(e.target)
        console.log("right clicked", $(e.target).attr("class"));
        $("#element").text( $(e.target).prop('tagName') )
        */
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
        placeholder: "border border-dotted p-4 bg-yellow-100",
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
              }
            })
          ],
          doc: ''
        }),
        parent: document.getElementById('code-mirror-editor')
      })

      $( document ).tooltip({
        classes: {
          "ui-tooltip": "bg-yellow-100 text-xs rounded-md border-none"
        },
        /*position: {
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
        }*/
      });

    });
  },
  watch: {
    selected_element() {
      if($(this.selected_element).prop("tagName") === 'INPUT'){
        this.selected_element_inner_html = $(this.selected_element).val()
      }
      else if($(this.selected_element).prop("tagName") === 'IMG'){
        this.selected_element_inner_html = $(this.selected_element).attr('src')
      }
      else{
        this.selected_element_inner_html = $(this.selected_element).html()
      }
    },
    selected_element_inner_html() {
      if($(this.selected_element).prop("tagName") === 'INPUT'){
        $(this.selected_element).val(this.selected_element_inner_html)
      }
      else if($(this.selected_element).prop("tagName") === 'IMG'){
        $(this.selected_element).attr('src', this.selected_element_inner_html)
      }
      else{
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
