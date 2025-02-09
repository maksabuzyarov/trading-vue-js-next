<template>
  <div
    class="code-pane"
    :style="{
        background: colors.back,
        borderColor: colors.border,
        width: width + 'px'
    }"
  >
    <codemirror
      ref="cm"
      v-model="code"
      :options="options"
      @input="on_change"
    />
  </div>
</template>

<script>

import { codemirror } from 'vue-codemirror';
import 'codemirror/mode/javascript/javascript';
import 'codemirror/mode/htmlmixed/htmlmixed';
import 'codemirror/lib/codemirror.css';
import 'codemirror/theme/dracula.css';
import 'codemirror/addon/scroll/simplescrollbars.js';
import 'codemirror/addon/scroll/simplescrollbars.css';

export default {
  name: 'Codepane',
  components: {
    codemirror,
  },
  props: ['colors', 'height', 'width', 'bundle', 'src'],
  emits: ['src-changed'],
  data() {
    return {
      code: this.$props.src,
      options: {
        mode: 'text/javascript',
        tabSize: 4,
        styleActiveLine: true,
        lineNumbers: true,
        line: true,
        foldGutter: true,
        styleSelectedText: true,
        runmode: 'colorize',
        matchBrackets: true,
        showCursorWhenSelecting: true,
        theme: 'dracula',
        extraKeys: {
          'Ctrl': 'autocomplete',
        },
        hintOptions: {
          completeSingle: false,
        },
        scrollbarStyle: 'simple',
      },
    };
  },
  computed: {
    style() {
      return `
                .CodeMirror {
                    height: ${this.$props.height - 50}px !important;
                }
                .cm-s-dracula.CodeMirror,
                .cm-s-dracula
                .CodeMirror-gutters {
                    background-color: ${this.$props.colors.cmBack} !important;
                    color: ${this.$props.colors.cmCode} !important;
                }
                .cm-property {
                    color: ${this.$props.colors.cmProperty} !important;
                }
                .cm-s-dracula .CodeMirror-linenumber {
                    color: ${this.$props.colors.cmLineNumber};
                }
                .cm-comment {
                    color: ${this.$props.colors.cmComment} !important;
                }
                .cm-attribute {
                    color: ${this.$props.colors.cmAttribute} !important;
                }
                .cm-tag,
                .cm-keyword {
                    color: ${this.$props.colors.cmKeyword} !important;
                }
                .CodeMirror-cursor {
                    border-left: solid thin ${this.$props.colors.text} !important;
                }
                .CodeMirror-selected {
                    background: ${this.$props.colors.selection} !important;
                }
            `;
    },
  },
  watch: {
    width() {
      if (!this.$refs.cm) return;
      this.$refs.cm.refresh();
    },
  },
  mounted() {
    this.reset_style();
  },
  methods: {
    reset_style() {
      var stbr = document.getElementById('code-mirror-custom');
      if (stbr) {
        var sheetParent = stbr.parentNode;
        sheetParent.removeChild(stbr);
      }


      var sheet = document.createElement('style');
      sheet.setAttribute('id', 'code-mirror-custom');
      sheet.innerHTML = this.style;
      document.body.appendChild(sheet);
    },
    on_change(text) {
      this.$emit('src-changed', text);
    },
  },
};
</script>

<style>
.code-pane {
  position: absolute;
  top: 0;
  height: 100%;
  right: 0;
  border-left: 1px solid;
}

.CodeMirror {
  font-size: 14px;
}

.cm-tag,
.cm-keyword {
  /*color: #dddcdc !important;*/
  /*color: #e27777 !important;*/
  color: #6DC59F !important;
}

.cm-variable,
.cm-variable-2,
.cm-def {
  /*color: #50fa7b !important;*/
  color: #3B9E64 !important;
  /* specific keyword #d0ca95 */
}

.cm-string {
  color: #9FAABB !important;
}

.cm-string-2 {
  color: #9FAABB !important;
}

.cm-operator {
  color: /*#e54150*/ #9A9A9A !important;
}

.cm-property {
  color: #35A776 !important;
}

.cm-number {
  color: #FBAB66 !important;
}

.cm-comment {
  color: #9098AF63 !important;
}

.cm-s-dracula.CodeMirror,
.cm-s-dracula
.CodeMirror-gutters {
  background-color: /*#121827*/ #1B202D !important;
}

.CodeMirror-simplescroll-vertical,
.CodeMirror-simplescroll-horizontal {
  background: none;
}

.CodeMirror-simplescroll-horizontal div,
.CodeMirror-simplescroll-vertical div {
  border-radius: 20px;
  border: none;
  background: #7B7B7B;
  opacity: 0.35;
  right: 1px;
}

.CodeMirror-simplescroll-horizontal div:hover,
.CodeMirror-simplescroll-vertical div:hover {
  filter: brightness(1.5);
}

.CodeMirror-scrollbar-filler, .CodeMirror-gutter-filler {
  background: none;
}

.CodeMirror-simplescroll-horizontal {
  bottom: 3px;
}
</style>
