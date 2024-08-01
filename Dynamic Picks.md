Follow these steps for Dynamic Pick in oracle apex:

1. Create a Page item below item (:P83_SUB in my case) where to place picks
   a. Tpe: Display Only
   b. Label Column Span: 0
   c. Pre Text:
         
        <div id="keywordButtons">
          <button type="button" onclick="insertKeyword('BRAND', 'P83_SUB')">Brand Name</button>
          <button type="button" onclick="insertKeyword('BNBRAND', 'P83_SUB')">Brand Name Bangla</button>
          <button type="button" onclick="insertKeyword('COMB', 'P83_SUB')">Combinations</button>
        </div>

2. Use this JS function in Global variable Declaration
   a. Paste this function and modify if needed.
   
         // Function to insert the selected keyword at the cursor position
        function insertKeyword(keyword, areaId) {
            // Get the CKEditor instance based on the areaId
            var editor = CKEDITOR.instances[areaId];
            
            // If the CKEditor instance exists for the given areaId, insert the keyword
            if (editor) {
                editor.insertText(keyword);
            } else {
                // If no CKEditor instance is found, insert into the regular text area
                var textarea = document.getElementById(areaId);
                var startPos = textarea.selectionStart;
                var endPos = textarea.selectionEnd;
                var text = textarea.value;
                var newText = text.substring(0, startPos) + keyword + text.substring(endPos, text.length);
                textarea.value = newText;
                // Set the cursor position after the inserted keyword
                textarea.setSelectionRange(startPos + keyword.length, startPos + keyword.length);
                textarea.focus();
            }
        }

3. In order to change values in Report

       SELECT REPLACE( REPLACE( REPLACE( :P83_SUB,'BNBRAND', vBRAND_BN ), 'BRAND',  vBRAND ),'COMB', vCOMB ) AS translated_sentence INTO V_SUB FROM dual;

replace picks value with variables you need.
