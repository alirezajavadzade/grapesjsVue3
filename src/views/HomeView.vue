<template>
  <div id="gjs" ref="gjs">
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import 'grapesjs/dist/css/grapes.min.css';
import grapesjs from 'grapesjs';
import gjsBlocksBasic from 'grapesjs-blocks-basic';
import grapesjsPluginForms from 'grapesjs-plugin-forms';
import grapesjsPluginExport from 'grapesjs-plugin-export';
import grapesjsPresetWebpage from 'grapesjs-preset-webpage';
import grapesjsTemplates from 'grapesjs-templates';
import grapesjsCustomCode from 'grapesjs-custom-code';
import grapesjsIndexeddb from 'grapesjs-indexeddb';
import fa from 'grapesjs/locale/fa';
import grapesjsRulers from 'grapesjs-rulers'
import 'grapesjs-rulers/dist/grapesjs-rulers.min.css'

const editor = ref(null);
const gjs = ref();


onMounted(() => {

  let x = document.querySelector('.gjs-pn-buttons')


  editor.value = grapesjs.init({
    height: '100%',
    container: gjs.value,
    showOffsets: true,
    fromElement: true,
    noticeOnUnload: false,
    dragMode: 'absolute',
    pageManager: true,
    deviceManager: {
      devices: [{
        name: 'A3',
        width: '1122px',
        widthMedia: '793px',
        height: '793px',
      }, {
        name: 'A4',
        width: '793px',
        widthMedia: '1122px',
        height: '1122px',
      },
      {
        name: 'A5',
        width: '561px',
        widthMedia: '793px',
        height: '793px',
      },
      ]
    },

    i18n: {
      locale: 'fa',
      localeFallback: 'fa',
      messages: { fa, fa },
    },
    selectorManager: {
      componentFirst: true,
    },
    storageManager: {
      type: "indexeddb",
      autosave: true,
      autoload: true,


      // type : "local",
      // autosave: true,
      // autoload: true,
      // options: {
      //   local: {
      //     key: 'gjsProject',
      //   },
      // }


      // type: 'rest-api', 
      // options: {
      //   remote: {
      //     urlStore: 'https://endpoint/store/',// POST
      //     urlLoad: 'https://endpoint/load/',// GET
      //     urlDelete: 'https://endpoint/delete/',// DELETE
      //     // ...
      //   }
      // }
    },
    plugins: [gjsBlocksBasic, grapesjsPluginForms, grapesjsPluginExport, grapesjsPresetWebpage, grapesjsCustomCode, grapesjsIndexeddb, grapesjsTemplates, grapesjsRulers],
    pluginsOpts: {
      gjsBlocksBasic: { flexGrid: true, blocks: ['text', 'link', 'image', 'video', 'map'] },
      grapesjsIndexeddb: {
        options: {
          key: 'user-project-id',
          dbName: 'editorLocalData',
          objectStoreName: 'projects',
        }
      },
      grapesjsPresetWebpage: {
        modalImportTitle: 'Import Template',
        modalImportLabel: '<div style="margin-bottom: 10px; font-size: 13px;">Paste here your HTML/CSS and click Import</div>',
        modalImportContent: function (editor) {
          return editor.getHtml() + '<style>' + editor.getCss() + '</style>'
        },
        grapesjsTemplates: {
          templates: 'http://localhost:5173/templates',
          projects: 'http://localhost:5173/projects',
        },
      },
    },
    blockManager: {
      blocks: [
        {
          id: 'footer',
          label: 'footer',
          category: 'constant',
          media: `<svg fill="#000000" version="1.1" id="Layer_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" viewBox="0 0 512 512" xml:space="preserve"><g><g><g><path d="M490.667,0H21.333C9.536,0,0,9.557,0,21.333V128h512V21.333C512,9.557,502.464,0,490.667,0z M85.333,106.667C73.557,106.667,64,97.109,64,85.333S73.557,64,85.333,64s21.333,9.557,21.333,21.333S97.109,106.667,85.333,106.667zM149.333,106.667c-11.776,0-21.333-9.557-21.333-21.333S137.557,64,149.333,64s21.333,9.557,21.333,21.333S161.109,106.667,149.333,106.667z M213.333,106.667c-11.776,0-21.333-9.557-21.333-21.333S201.557,64,213.333,64s21.333,9.557,21.333,21.333S225.109,106.667,213.333,106.667z"/><rect y="170.667" width="512" height="192"/><path d="M0,490.667C0,502.443,9.536,512,21.333,512h469.333c11.797,0,21.333-9.557,21.333-21.333v-85.333H0V490.667z"/></g></g></g></svg>`,
          content: {
            type: 'div',
            attributes: { class: 'footer' },
            components: [{
              type: 'div',
              content: 'Hello world!!!'
            }, {
              type: 'div',
              content: 'Hello world!!!'
            }],
            styles: `.footer { width:100%; color:red; }`,
          },
          activate: true,
        }
      ],
    }
  });





  const pn = editor.value.Panels;
  const panelViews = pn.addPanel({
    id: 'options'
  });
  panelViews.get('buttons').add([{
    attributes: {
      title: 'Toggle Rulers'
    },
    context: 'toggle-rulers',
    label: `<svg width="18" viewBox="0 0 16 16"><path d="M0 8a.5.5 0 0 1 .5-.5h15a.5.5 0 0 1 0 1H.5A.5.5 0 0 1 0 8z"/><path d="M4 3h8a1 1 0 0 1 1 1v2.5h1V4a2 2 0 0 0-2-2H4a2 2 0 0 0-2 2v2.5h1V4a1 1 0 0 1 1-1zM3 9.5H2V12a2 2 0 0 0 2 2h8a2 2 0 0 0 2-2V9.5h-1V12a1 1 0 0 1-1 1H4a1 1 0 0 1-1-1V9.5z"/></svg>`,
    command: 'ruler-visibility',
    id: 'ruler-visibility'
  }]);





  editor.value.Commands.add('set-device-desktop', {
    run: editor => editor.setDevice('A3')
  });
  editor.value.Commands.add('set-device-tablet', {
    run: editor => editor.setDevice('A4')
  });
  editor.value.Commands.add('set-device-mobile', {
    run: editor => editor.setDevice('A5')
  });
})


</script>

<style>
#gjs {
  height: 100%;
}

@media print {
  .gjs-pn-panels {
    display: none;
  }
}
</style>