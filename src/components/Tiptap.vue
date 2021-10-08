<template>
    <div class='toolbar h-50' v-if='true'>
      <button class="btn" @click="editor.chain().focus().toggleBold().run()">
        bold
      </button>
      <button class="btn" @click="editor.chain().focus().toggleItalic().run()" :class="{ 'is-active': editor.isActive('italic') }">
        italic
      </button>
      <button class="btn" @click="editor.chain().focus().toggleStrike().run()" :class="{ 'is-active': editor.isActive('strike') }">
        strike
      </button>
      <button class="btn" @click="editor.chain().focus().toggleCode().run()" :class="{ 'is-active': editor.isActive('code') }">
        code
      </button>
      <button class="btn" @click="editor.chain().focus().unsetAllMarks().run()">
        clear marks
      </button>
      <button class="btn" @click="editor.chain().focus().clearNodes().run()">
        clear nodes
      </button>
      <button class="btn" @click="editor.chain().focus().setParagraph().run()" :class="{ 'is-active': editor.isActive('paragraph') }">
        paragraph
      </button>
      <button class="btn" @click="editor.chain().focus().toggleHeading({ level: 1 }).run()" :class="{ 'is-active': editor.isActive('heading', { level: 1 }) }">
        h1
      </button>
      <button class="btn" @click="editor.chain().focus().toggleHeading({ level: 2 }).run()" :class="{ 'is-active': editor.isActive('heading', { level: 2 }) }">
        h2
      </button>
      <button class="btn" @click="editor.chain().focus().toggleHeading({ level: 3 }).run()" :class="{ 'is-active': editor.isActive('heading', { level: 3 }) }">
        h3
      </button>
      <button class="btn" @click="editor.chain().focus().toggleHeading({ level: 4 }).run()" :class="{ 'is-active': editor.isActive('heading', { level: 4 }) }">
        h4
      </button>
      <button class="btn" @click="editor.chain().focus().toggleHeading({ level: 5 }).run()" :class="{ 'is-active': editor.isActive('heading', { level: 5 }) }">
        h5
      </button>
      <button class="btn" @click="editor.chain().focus().toggleHeading({ level: 6 }).run()" :class="{ 'is-active': editor.isActive('heading', { level: 6 }) }">
        h6
      </button>
      <button class="btn" @click="editor.chain().focus().toggleBulletList().run()" :class="{ 'is-active': editor.isActive('bulletList') }">
        bullet list
      </button>
      <button class="btn" @click="editor.chain().focus().toggleOrderedList().run()" :class="{ 'is-active': editor.isActive('orderedList') }">
        ordered list
      </button>
      <button class="btn" @click="editor.chain().focus().toggleCodeBlock().run()" :class="{ 'is-active': editor.isActive('codeBlock') }">
        code block
      </button>
      <button class="btn" @click="editor.chain().focus().toggleBlockquote().run()" :class="{ 'is-active': editor.isActive('blockquote') }">
        blockquote
      </button>
      <button class="btn" @click="editor.chain().focus().setHorizontalRule().run()">
        horizontal rule
      </button>
      <button class="btn" @click="editor.chain().focus().setHardBreak().run()">
        hard break
      </button>
      <button class="btn" @click="editor.chain().focus().undo().run()">
        undo
      </button>
      <button class="btn" @click="editor.chain().focus().redo().run()">
        redo
      </button>
      <button class="btn" v-on:click="fireAlert">Submit</button>
    </div>

    <div>
      <editor-content class='fullheight' :editor="editor" />
    </div>
</template>

<script lang='ts'>
import { Editor, EditorContent } from '@tiptap/vue-3'
import CharacterCount from '@tiptap/extension-character-count'
import StarterKit from '@tiptap/starter-kit'
import { defineComponent } from 'vue'

const editor = new Editor({
  content: '<p>Begin typing here </p>',
  extensions: [
    StarterKit,
    CharacterCount,
  ],
  onUpdate({ editor }) {
    const wordCount = editor.state.doc.textContent.length
    console.log("word count:", wordCount)
    if(wordCount >= 30 ){
      editor.options.editable = false
      alert(`Character limit is ${ wordCount }`)
    }
  }
})

const Component = defineComponent({
  computed: {
    boldclass(): boolean {
      try {
        return this.editor.isActive('bold')
      } catch (e) {
        console.log(e)
      }
      console.log("this.editor", this.editor)
      return false
    },
    deactivate: function() {
      try {
        return this.editor.isActive(false)
      } catch (e) {
        console.log(e)
      }
    },
  },

  components: {
    EditorContent,
  },

  props:
    ['editable'],

  methods: {
    currentDate() {
      const current = new Date();
      const date = `${current.getDate()}/${current.getMonth()+1}/${current.getFullYear()}`;
      return date;
    },
    fireAlert: function() {
      alert('Submit button clicked')
    }
  },

  data() {
    return {
      editor: editor,
      date: new Date().toISOString(),
      limit: 20,
    }
  },

  // mounted() {
  //   // this.editor = editor
  //   // console.log("console log this.editor in mounted", this.editor)

  //   this.editor = new Editor({
  //     content: '<p>What is this?</p>',
  //     extensions: [
  //       StarterKit,
  //       CharacterCount.configure({
  //         limit: this.limit,
  //       }),
  //     ],
  //   })
  // },

  beforeUnmount() {
    this.editor.destroy()
  },
})

export default Component

</script>
