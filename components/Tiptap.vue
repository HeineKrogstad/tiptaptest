<template>
<div v-if="editor">
    <div>
    <div class="flex justify-center space-x-2 p-1">
        <button class="btn" @click="editor.chain().focus().toggleHeading({ level: 1 }).run()" :class="{ 'is-active': editor.isActive('heading', { level: 1 }) }">
        H1
        </button>
        <button class="btn" @click="editor.chain().focus().toggleHeading({ level: 2 }).run()" :class="{ 'is-active': editor.isActive('heading', { level: 2 }) }">
        H2
        </button>
        <button class="btn" @click="editor.chain().focus().toggleHeading({ level: 3 }).run()" :class="{ 'is-active': editor.isActive('heading', { level: 3 }) }">
        H3
        </button>
        <button class="btn" @click="editor.chain().focus().setParagraph().run()" :class="{ 'is-active': editor.isActive('paragraph') }">
        Paragraph
        </button>
        <button class="btn" @click="editor.chain().focus().toggleBold().run()" :class="{ 'is-active': editor.isActive('bold') }">
        Bold
        </button>
        <button class="btn" @click="editor.chain().focus().toggleItalic().run()" :class="{ 'is-active': editor.isActive('italic') }">
        Italic
        </button>
        <button class="btn" @click="editor.chain().focus().toggleStrike().run()" :class="{ 'is-active': editor.isActive('strike') }">
        Strike
        </button>
        <button class="btn" @click="editor.chain().focus().toggleHighlight().run()" :class="{ 'is-active': editor.isActive('highlight') }">
        Highlight
        </button>
        <button class="btn" @click="editor.chain().focus().setTextAlign('left').run()" :class="{ 'is-active': editor.isActive({ textAlign: 'left' }) }">
        Left
        </button>
        <button class="btn" @click="editor.chain().focus().setTextAlign('center').run()" :class="{ 'is-active': editor.isActive({ textAlign: 'center' }) }">
        Center
        </button>
        <button class="btn" @click="editor.chain().focus().setTextAlign('right').run()" :class="{ 'is-active': editor.isActive({ textAlign: 'right' }) }">
        Right
        </button>
        <button class="btn" @click="editor.chain().focus().setTextAlign('justify').run()" :class="{ 'is-active': editor.isActive({ textAlign: 'justify' }) }">
        Justify
        </button>
        <button class="btn-export" @click="exportToJson">Получить JSON</button>
    </div>
    </div>
    <editor-content :editor="editor"/>
</div>
</template>
  
<script>
import Highlight from '@tiptap/extension-highlight'
import TextAlign from '@tiptap/extension-text-align'
import StarterKit from '@tiptap/starter-kit'
import { Editor, EditorContent } from '@tiptap/vue-3'

export default {
    components: {
        EditorContent,
    },

    data() {
        return {
        editor: null,
        }
    },

    mounted() {
        this.editor = new Editor({
        extensions: [
            StarterKit,
            TextAlign.configure({
            types: ['heading', 'paragraph'],
            }),
            Highlight,
        ],
        content: ``,
        })
    },
    
    beforeUnmount() {
        this.editor.destroy()
    },

    methods: {
        exportToJson() {
            const json = this.editor.getJSON();
            const jsonString = JSON.stringify(json, null, 2);
            const blob = new Blob([jsonString], { type: 'application/json' });
            const url = URL.createObjectURL(blob);
            const newWindow = window.open(url);
            newWindow.onload = () => URL.revokeObjectURL(url);
        },
    },
}
</script>

<style>
.btn {
    @apply bg-zinc-950 hover:bg-zinc-800 text-white font-mono py-1 px-2 rounded-lg transition-colors duration-300;
}

.btn.is-active {
    @apply bg-slate-500;
}

.tiptap {
    @apply m-8 p-8 border border-gray-300 rounded-lg shadow-sm;
    @apply font-normal text-gray-800;
}   
    
h1, h2 { @apply mt-3.5 mb-1.5; }

h1 { @apply text-2xl }

h2 { @apply text-xl }

h3 { @apply text-lg }

.btn-export {
    @apply border border-gray-300 rounded-lg p-1 hover:bg-gray-200 transition-colors duration-300;
}
</style>
