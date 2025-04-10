<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Scripture File Viewer</title>
  <style>
    :root {
      --primary-color: #4a5568;
      --bg-color: #f7fafc;
      --text-color: #2d3748;
      --border-color: #e2e8f0;
      --highlight-color: #3182ce;
    }
    
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      margin: 0;
      padding: 0;
      background-color: var(--bg-color);
      color: var(--text-color);
    }
    
    .container {
      max-width: 1200px;
      margin: 0 auto;
      padding: 1rem;
    }
    
    header {
      background-color: var(--primary-color);
      color: white;
      padding: 1rem;
      border-radius: 0.5rem;
      margin-bottom: 1rem;
    }
    
    h1 {
      margin: 0;
      font-size: 1.5rem;
    }
    
    .toolbar {
      display: flex;
      flex-wrap: wrap;
      gap: 1rem;
      margin-bottom: 1rem;
      padding: 1rem;
      background-color: white;
      border-radius: 0.5rem;
      box-shadow: 0 1px 3px rgba(0,0,0,0.1);
    }
    
    .toolbar-section {
      display: flex;
      flex-direction: column;
      gap: 0.5rem;
    }
    
    .file-controls {
      display: flex;
      gap: 0.5rem;
      flex-wrap: wrap;
    }
    
    button, select, input[type="color"] {
      padding: 0.5rem;
      border: 1px solid var(--border-color);
      border-radius: 0.25rem;
      background-color: white;
      cursor: pointer;
    }
    
    button:hover {
      background-color: #edf2f7;
    }
    
    label {
      font-size: 0.875rem;
      font-weight: 600;
    }
    
    .panels-container {
      display: flex;
      gap: 1rem;
      flex-wrap: wrap;
    }
    
    .panel {
      flex: 1;
      min-width: 300px;
      height: 600px;
      border: 1px solid var(--border-color);
      border-radius: 0.5rem;
      background-color: white;
      overflow: hidden;
      display: flex;
      flex-direction: column;
    }
    
    .panel-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 0.5rem;
      background-color: #edf2f7;
      border-bottom: 1px solid var(--border-color);
    }
    
    .panel-title {
      font-weight: 600;
      font-size: 0.875rem;
    }
    
    .panel-content {
      padding: 1rem;
      overflow: auto;
      flex: 1;
    }
    
    .custom-font-modal {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(0,0,0,0.5);
      display: flex;
      justify-content: center;
      align-items: center;
      z-index: 1000;
      display: none;
    }
    
    .modal-content {
      background-color: white;
      border-radius: 0.5rem;
      padding: 1.5rem;
      width: 90%;
      max-width: 500px;
    }
    
    .modal-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 1rem;
    }
    
    .modal-close {
      background: none;
      border: none;
      font-size: 1.5rem;
      cursor: pointer;
    }
    
    .form-group {
      margin-bottom: 1rem;
    }
    
    .form-group label {
      display: block;
      margin-bottom: 0.5rem;
    }
    
    .form-group input {
      width: 100%;
      padding: 0.5rem;
      border: 1px solid var(--border-color);
      border-radius: 0.25rem;
    }
    
    .btn-primary {
      background-color: var(--highlight-color);
      color: white;
      border: none;
      padding: 0.5rem 1rem;
      border-radius: 0.25rem;
      cursor: pointer;
    }
    
    .rtl {
      direction: rtl;
      text-align: right;
    }
    
    .ltr {
      direction: ltr;
      text-align: left;
    }
    
    @media (max-width: 768px) {
      .toolbar {
        flex-direction: column;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <header>
      <h1>Scripture File Viewer</h1>
    </header>
    
    <div class="toolbar">
      <div class="toolbar-section">
        <label>Add Files</label>
        <div class="file-controls">
          <input type="file" id="fileInput" accept=".sfm,.usx,.xml" multiple>
          <button id="addPanelBtn">Add Empty Panel</button>
        </div>
      </div>
      
      <div class="toolbar-section">
        <label>Text Direction</label>
        <select id="textDirection">
          <option value="ltr">Left to Right</option>
          <option value="rtl">Right to Left</option>
        </select>
      </div>
      
      <div class="toolbar-section">
        <label>Font</label>
        <select id="fontSelector">
          <option value="Arial, sans-serif">Arial</option>
          <option value="'Times New Roman', serif">Times New Roman</option>
          <option value="'Courier New', monospace">Courier New</option>
          <option value="Georgia, serif">Georgia</option>
          <option value="Verdana, sans-serif">Verdana</option>
          <option value="'SBL Hebrew', serif">SBL Hebrew</option>
          <option value="'Noto Sans', sans-serif">Noto Sans</option>
        </select>
        <button id="addFontBtn">Add Custom Font</button>
      </div>
      
      <div class="toolbar-section">
        <label>Font Size</label>
        <select id="fontSize">
          <option value="12px">12px</option>
          <option value="14px" selected>14px</option>
          <option value="16px">16px</option>
          <option value="18px">18px</option>
          <option value="20px">20px</option>
          <option value="24px">24px</option>
        </select>
      </div>
      
      <div class="toolbar-section">
        <label>Text Color</label>
        <input type="color" id="textColor" value="#2d3748">
      </div>
      
      <div class="toolbar-section">
        <label>Background Color</label>
        <input type="color" id="bgColor" value="#ffffff">
      </div>
    </div>
    
    <div class="panels-container" id="panelsContainer">
      <!-- Panels will be added here -->
    </div>
  </div>
  
  <div class="custom-font-modal" id="customFontModal">
    <div class="modal-content">
      <div class="modal-header">
        <h2>Add Custom Font</h2>
        <button class="modal-close" id="closeModal">&times;</button>
      </div>
      <div class="form-group">
        <label for="fontName">Font Name</label>
        <input type="text" id="fontName" placeholder="e.g., My Custom Font">
      </div>
      <div class="form-group">
        <label for="fontFile">Font File (optional)</label>
        <input type="file" id="fontFile" accept=".ttf,.otf,.woff,.woff2">
      </div>
      <div class="form-group">
        <label for="fontUrl">OR Font URL</label>
        <input type="text" id="fontUrl" placeholder="e.g., https://fonts.googleapis.com/css2?family=...">
      </div>
      <button class="btn-primary" id="saveFontBtn">Add Font</button>
    </div>
  </div>

  <script>
    document.addEventListener('DOMContentLoaded', function() {
      // DOM Elements
      const fileInput = document.getElementById('fileInput');
      const addPanelBtn = document.getElementById('addPanelBtn');
      const panelsContainer = document.getElementById('panelsContainer');
      const textDirection = document.getElementById('textDirection');
      const fontSelector = document.getElementById('fontSelector');
      const fontSize = document.getElementById('fontSize');
      const textColor = document.getElementById('textColor');
      const bgColor = document.getElementById('bgColor');
      const addFontBtn = document.getElementById('addFontBtn');
      const customFontModal = document.getElementById('customFontModal');
      const closeModal = document.getElementById('closeModal');
      const saveFontBtn = document.getElementById('saveFontBtn');
      
      // Custom fonts storage
      const customFonts = [];
      let panelCounter = 0;
      
      // Event Listeners
      fileInput.addEventListener('change', handleFileSelection);
      addPanelBtn.addEventListener('click', addEmptyPanel);
      textDirection.addEventListener('change', updateTextDirection);
      fontSelector.addEventListener('change', updateFont);
      fontSize.addEventListener('change', updateFontSize);
      textColor.addEventListener('input', updateTextColor);
      bgColor.addEventListener('input', updateBackgroundColor);
      addFontBtn.addEventListener('click', showCustomFontModal);
      closeModal.addEventListener('click', hideCustomFontModal);
      saveFontBtn.addEventListener('click', addCustomFont);
      
      // Add initial empty panel
      addEmptyPanel();
      
      // Functions
      function handleFileSelection(event) {
        const files = event.target.files;
        if (!files.length) return;
        
        Array.from(files).forEach(file => {
          const fileName = file.name;
          const reader = new FileReader();
          
          reader.onload = function(e) {
            const content = e.target.result;
            addPanel(fileName, content);
          };
          
          reader.readAsText(file);
        });
        
        // Reset file input
        fileInput.value = '';
      }
      
      function addEmptyPanel() {
        addPanel('Empty Panel', '');
      }
      
      function addPanel(title, content) {
        panelCounter++;
        const panelId = `panel-${panelCounter}`;
        
        const panel = document.createElement('div');
        panel.className = 'panel';
        panel.id = panelId;
        
        const currentDirection = textDirection.value;
        const currentFont = fontSelector.value;
        const currentFontSize = fontSize.value;
        const currentTextColor = textColor.value;
        const currentBgColor = bgColor.value;
        
        panel.innerHTML = `
          <div class="panel-header">
            <div class="panel-title">${title}</div>
            <button class="panel-close" onclick="document.getElementById('${panelId}').remove()">&times;</button>
          </div>
          <div class="panel-content ${currentDirection}" 
               style="font-family: ${currentFont}; 
                      font-size: ${currentFontSize};
                      color: ${currentTextColor};
                      background-color: ${currentBgColor};">
            ${parseFileContent(content, title)}
          </div>
        `;
        
        panelsContainer.appendChild(panel);
      }
      
      function parseFileContent(content, fileName) {
        if (!content) return '';
        
        // Determine file type from extension
        const extension = fileName.split('.').pop().toLowerCase();
        
        try {
          if (extension === 'usx' || extension === 'xml') {
            return parseUSXContent(content);
          } else if (extension === 'sfm') {
            return parseSFMContent(content);
          } else {
            // If we can't determine the type, just display as plain text
            return `<pre>${escapeHTML(content)}</pre>`;
          }
        } catch (error) {
          return `<div class="error">Error parsing file: ${error.message}</div>`;
        }
      }
      
      function parseUSXContent(content) {
        // Create a DOM parser for XML
        const parser = new DOMParser();
        const xmlDoc = parser.parseFromString(content, "text/xml");
        
        // Check for parsing errors
        if (xmlDoc.getElementsByTagName('parsererror').length > 0) {
          return `<div class="error">Invalid XML format</div><pre>${escapeHTML(content)}</pre>`;
        }
        
        // Simple USX rendering - this would need to be expanded for full USX support
        let html = '';
        
        // Handle scripture text
        const chapters = xmlDoc.getElementsByTagName('chapter');
        const verses = xmlDoc.getElementsByTagName('verse');
        const paras = xmlDoc.getElementsByTagName('para');
        
        // Process paragraphs
        for (let i = 0; i < paras.length; i++) {
          const para = paras[i];
          const style = para.getAttribute('style') || '';
          
          if (style.startsWith('h')) {
            // Heading
            html += `<h${style.charAt(1) || '2'}>${para.textContent}</h${style.charAt(1) || '2'}>`;
          } else if (style === 'p') {
            // Regular paragraph
            html += `<p>${processNodes(para.childNodes)}</p>`;
          } else {
            // Other paragraph types
            html += `<div class="para ${style}">${processNodes(para.childNodes)}</div>`;
          }
        }
        
        return html || `<pre>${escapeHTML(content)}</pre>`;
      }
      
      function processNodes(nodes) {
        let result = '';
        
        for (let i = 0; i < nodes.length; i++) {
          const node = nodes[i];
          
          if (node.nodeType === Node.TEXT_NODE) {
            result += node.textContent;
          } else if (node.nodeType === Node.ELEMENT_NODE) {
            if (node.tagName === 'verse') {
              const num = node.getAttribute('number') || '';
              result += `<sup class="verse-number">${num}</sup> `;
            } else if (node.tagName === 'chapter') {
              const num = node.getAttribute('number') || '';
              result += `<strong class="chapter-number">${num}</strong> `;
            } else {
              result += processNodes(node.childNodes);
            }
          }
        }
        
        return result;
      }
      
      function parseSFMContent(content) {
        // Simple SFM parsing - would need to be expanded for full SFM support
        let html = '';
        const lines = content.trim().split('\n');
        
        // Process paragraph markers and verses
        let currentParagraph = '';
        let inParagraph = false;
        
        lines.forEach(line => {
          const trimmed = line.trim();
          
          // Chapter marker
          if (trimmed.startsWith('\\c ')) {
            if (inParagraph) {
              html += `<p>${currentParagraph}</p>`;
              currentParagraph = '';
              inParagraph = false;
            }
            const chapter = trimmed.substring(3).trim();
            html += `<h2 class="chapter">Chapter ${chapter}</h2>`;
          }
          // Verse marker
          else if (trimmed.startsWith('\\v ')) {
            const verseText = trimmed.substring(3).trim();
            const verseNum = verseText.split(' ')[0];
            const verseContent = verseText.substring(verseNum.length).trim();
            
            if (!inParagraph) {
              inParagraph = true;
            }
            
            currentParagraph += `<sup class="verse-num">${verseNum}</sup> ${verseContent} `;
          }
          // Section heading
          else if (trimmed.startsWith('\\s ')) {
            if (inParagraph) {
              html += `<p>${currentParagraph}</p>`;
              currentParagraph = '';
              inParagraph = false;
            }
            html += `<h3>${trimmed.substring(3).trim()}</h3>`;
          }
          // Paragraph marker
          else if (trimmed.startsWith('\\p')) {
            if (inParagraph) {
              html += `<p>${currentParagraph}</p>`;
              currentParagraph = '';
            }
            inParagraph = true;
          }
          // Other markers or continuation of content
          else {
            if (inParagraph) {
              currentParagraph += ' ' + trimmed;
            } else if (trimmed) {
              html += `<div>${trimmed}</div>`;
            }
          }
        });
        
        // Add final paragraph if exists
        if (inParagraph && currentParagraph) {
          html += `<p>${currentParagraph}</p>`;
        }
        
        return html || `<pre>${escapeHTML(content)}</pre>`;
      }
      
      function updateTextDirection() {
        const direction = textDirection.value;
        const panelContents = document.querySelectorAll('.panel-content');
        
        panelContents.forEach(content => {
          content.classList.remove('rtl', 'ltr');
          content.classList.add(direction);
        });
      }
      
      function updateFont() {
        const selectedFont = fontSelector.value;
        const panelContents = document.querySelectorAll('.panel-content');
        
        panelContents.forEach(content => {
          content.style.fontFamily = selectedFont;
        });
      }
      
      function updateFontSize() {
        const size = fontSize.value;
        const panelContents = document.querySelectorAll('.panel-content');
        
        panelContents.forEach(content => {
          content.style.fontSize = size;
        });
      }
      
      function updateTextColor() {
        const color = textColor.value;
        const panelContents = document.querySelectorAll('.panel-content');
        
        panelContents.forEach(content => {
          content.style.color = color;
        });
      }
      
      function updateBackgroundColor() {
        const color = bgColor.value;
        const panelContents = document.querySelectorAll('.panel-content');
        
        panelContents.forEach(content => {
          content.style.backgroundColor = color;
        });
      }
      
      function showCustomFontModal() {
        customFontModal.style.display = 'flex';
      }
      
      function hideCustomFontModal() {
        customFontModal.style.display = 'none';
        document.getElementById('fontName').value = '';
        document.getElementById('fontFile').value = '';
        document.getElementById('fontUrl').value = '';
      }
      
      function addCustomFont() {
        const fontName = document.getElementById('fontName').value.trim();
        const fontFile = document.getElementById('fontFile').files[0];
        const fontUrl = document.getElementById('fontUrl').value.trim();
        
        if (!fontName) {
          alert('Please enter a font name');
          return;
        }
        
        if (!fontFile && !fontUrl) {
          alert('Please provide either a font file or a URL');
          return;
        }
        
        if (fontFile) {
          // Handle font file
          const fontUrl = URL.createObjectURL(fontFile);
          addFontFace(fontName, fontUrl);
        } else if (fontUrl) {
          // Handle font URL
          loadExternalFont(fontName, fontUrl);
        }
        
        // Add to select dropdown
        const option = document.createElement('option');
        option.value = `'${fontName}', sans-serif`;
        option.textContent = fontName;
        fontSelector.appendChild(option);
        
        // Select the new font
        fontSelector.value = `'${fontName}', sans-serif`;
        updateFont();
        
        // Close modal
        hideCustomFontModal();
      }
      
      function addFontFace(fontName, url) {
        const fontFace = new FontFace(fontName, `url(${url})`);
        
        fontFace.load().then(function(loadedFace) {
          document.fonts.add(loadedFace);
          customFonts.push({
            name: fontName,
            url: url
          });
        }).catch(function(error) {
          alert(`Error loading font: ${error.message}`);
        });
      }
      
      function loadExternalFont(fontName, url) {
        const linkEl = document.createElement('link');
        linkEl.rel = 'stylesheet';
        linkEl.href = url;
        document.head.appendChild(linkEl);
        
        customFonts.push({
          name: fontName,
          url: url
        });
      }
      
      function escapeHTML(str) {
        return str
          .replace(/&/g, '&amp;')
          .replace(/</g, '&lt;')
          .replace(/>/g, '&gt;')
          .replace(/"/g, '&quot;')
          .replace(/'/g, '&#039;');
      }
    });
  </script>
</body>
</html>
