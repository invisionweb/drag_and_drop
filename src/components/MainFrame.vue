<template>
  <div class="flex items-center p-4 gap-4">
    <img
      src="https://tailwindcss.com/_next/static/media/tailwindcss-logotype.128b6e12eb85d013bc9f80a917f57efe.svg"
      alt="TailwindCSS"
      class="h-6 w-auto"
    />
    <h1 class="text-2xl font-medium text-gray-500">Builder</h1>
    <button @click="clear_canvas">Clear</button>
  </div>
  <div class="grid grid-cols-8">
    <div class="flex flex-col col-span-1">
      <!-- <div class="draggable cursor-move select-none">Container</div> -->
      <div class="draggable cursor-move select-none" id="text">Text</div>
      <div class="draggable cursor-move select-none" id="button">Button</div>
      <div class="draggable cursor-move select-none" id="input">
        Input field
      </div>
      <!-- <div class="draggable cursor-move select-none" id="grid">Grid</div> -->
      <div class="draggable cursor-move select-none" id="flex">Flex</div>
      <div class="draggable cursor-move select-none" id="img">Image</div>
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
      <input class="border border-green-500 outline-none p-2" type="text" placeholder="Classes">
      <textarea class="border border-black hidden" v-model="selected_element_html" @keyup="element_html_change" id="element-html"></textarea>
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
      <div>Background color</div>
      <div>Text color</div>
      <div>Text size</div>
      <div>Shadow</div>
      <div>Radius</div>
      <div>Position</div>
      <div>
        Flex
        <select>
          <option>flex-row</option>
          <option>flex-col</option>
        </select>
      </div>
    </div>
  </div>
  <div
    contenteditable
    class="w-full border rounded-md resize"
    id="code-editor"
  ></div>
  <div class="border rounded-md m-6" id="code_editor"></div>
</template>

<script>
import $ from "jquery";
import "jquery-ui/ui/widgets/draggable";
import "jquery-ui/ui/widgets/droppable";
import "jquery-ui/ui/widgets/sortable";
import "jquery-ui/ui/widgets/resizable";
import "jquery-ui/themes/base/resizable.css";
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

export default {
  name: "MainFrame",
  data() {
    return {
      dragging_element_name: "",
      code_editor: null,
      selected_element: null,
      selected_element_html: ""
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
      }

      console.log(class_to_add, classes);
      $(this.selected_element).attr("class", classes);

      this.selected_element_html = $(this.selected_element).prop("outerHTML")
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

      clear_canvas(){
        $("#draggable-items-container").html("")
      },
    element_html_change(){
        if(! /<(?=.*? .*?\/ ?>|br|hr|input|!--|wbr)[a-z]+.*?>|<([a-z]+).*?<\/\1>/.test(this.selected_element_html)){
          return
        }
      console.log('element_html_change', this.selected_element_html)
      let new_elem = $(this.selected_element_html)
      new_elem.insertAfter($(this.selected_element))
      $(this.selected_element).remove()
      this.selected_element = new_elem
    },
    create_dropped_element(){
        let self = this
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
      }else if (self.dragging_element_name === "img") {
        element = document.createElement("img");
        $(element)
            .attr('src', "https://tailwindcss.com/_next/static/media/tailwindcss-logotype.128b6e12eb85d013bc9f80a917f57efe.svg")
            .addClass("m-2");
      }
      else if (self.dragging_element_name === "flex") {
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
              beforeStop: function(event, ui) {

                console.log("beforeStop", event, ui);
                console.log($(ui.item).hasClass("draggable"))

                if (!$(ui.item).hasClass("draggable")) { //self.dragging_element_name === ''
                  return
                }

                $(ui.item).replaceWith(self.create_dropped_element());

                $("#code-editor").text(js_beautify.html($(this).html()))
              }
            })
        //.resizable();
      } else if (self.dragging_element_name === "grid") {
        element = document.createElement("div");
        $(element)
            .addClass("grid grid-cols-3 gap-4 p-2 border")
            .droppable({
              drop: function() {
                console.log("grid drop");
              },
            });
      }

      //$(this).append(this.create_dropped_element(self.dragging_element_name)); //element
      $(element).contextmenu(function() {
        /* self.selected_element = $(e.target)
        console.log("right clicked", $(e.target).attr("class"));
        $("#element").text( $(e.target).prop('tagName') )
        */
        return false;
      });

      $(element).click(function(e) {
        self.selected_element = e.target
        console.log("left clicked", $(e.target).attr("class"));
        $("#element").text( $(e.target).prop('tagName') ).removeClass("hidden")
        //$("#element-html").val($(e.target).prop('outerHTML'))
        self.selected_element_html = $(e.target).prop('outerHTML')
        return false;
      });

      return element
    }
  },
  mounted() {
    let self = this;

    $(function() {
      /* $(".draggable").draggable({
        helper: "clone",
        start: function() {
          self.dragging_element_name = $(this).attr("id");
        },
      }); */

      $(".draggable").draggable({
        connectToSortable: "#draggable-items-container, .droppable-flex-container",
        helper: "clone",
        //revert: "invalid",
        start: function() {
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
        beforeStop: function(event, ui) {

          console.log("beforeStop", event, ui);
          console.log($(ui.item).hasClass("draggable"))

          if(! $(ui.item).hasClass("draggable")){ //self.dragging_element_name === ''
            return
          }

          $(ui.item).replaceWith(self.create_dropped_element());

          $("#code-editor").text(js_beautify.html( $(this).html(),
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
      /* .droppable({
        drop: function( event, ui ) {
           console.log('drop - ', event, ui)
        }
    }) */

      $(".droppable").droppable({
        accept: ".draggable",
        drop: function(event, ui) {
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
                drop: function(event, ui) {
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
                  $(child_element).contextmenu(function(e) {
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
                drop: function() {
                  console.log("grid drop");
                },
              });
          }

          $(this).append(element);
          $(element).contextmenu(function() {
            /* self.selected_element = e.target
            console.log("right clicked", $(e.target).attr("class"));
            $("#element").text( $(e.target).prop('tagName') )
            */
            return false;
          });

          $(element).click(function(e) {
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

      self.code_editor.on("change", function(code_mirror_element) {
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
    });
  },
  watch: {
    /*selected_element_html(){
      let self = this
      console.log($(this.selected_element))
      setTimeout(function (){
        console.log(self.selected_element_html)

        //$(self.selected_element).replaceWith(self.selected_element_html)
      }, 3000)
    }*/
  }
};
</script>
