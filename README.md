# CA-selectTheme
On 8/12/2021, I was testing a theme switcher in [my testing sandbox](https://riosalado.coursearc.com/content/sandbox-murz-testing-course/test-all-the-things/icon-and-text-block/) that we've been working on and I was confused that there were two switcher options. 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ![screenshot showing CourseArc (top) and Rio's (bottom) theme preview select menus](https://github.com/murrayinman/CA-selectTheme/blob/192403e3f9bcecb31f156aafbb741f1a41c0cbcd/twoThemeSelectors.png)

First, we verified that it wasn't our code doing it. Then I tried to reproduce it in different tabs and was unsuccessful. The html that was on the page was:
```html
<div class="cms_sidebar_block" id="cms_preview_block">
  <div class="cms-dialog-title">
    <span class="ui-dialog-title">Preview</span>
  </div>
  <form action="#">
    <label>Select theme: </label
    ><select name="previewPageTheme" id="previewPageTheme" ccm-passed-value="10179" class="ccm-input-select">
      <option value="10290">AAA Default theme</option>
      <option value="10138">ACCESSIBILITY RIO</option>
<!-- Full option list truncated for brevity -->
      <option value="10052">[IMPORT] Issue Checking Theme</option>
      <option value="10179" selected="selected">[MUR] DEV (Presentation Style)</option>
      <option value="10057">[RIO] Default (Blog Style)</option>
      <option value="10055">[RIO] Default (Presentation Style)</option>
    </select>
    <a
      class="btn"
      id="previewPageThemeButton"
      href="https://riosalado.coursearc.com/content/sandbox-murz-testing-course/test-all-the-things/icon-and-text-block/?vtask=view_versions&amp;themeCustomizationId=10138"
      target="_blank"
      >Preview</a
    >
  </form>
</div>

```

I found the part of the script in [page-edit.js](https://riosalado.coursearc.com/js/page-edit.js?version=8954b24f10b1a397731d36b7480636d37e3139b3) that handled the switching functions:
```javascript
/* Sidebar Preview */
$("#previewPageTheme").change(function () {
  $newPreviewUrl = replaceUrlParam($("#previewPageThemeButton").prop("href"), "themeCustomizationId", $("#previewPageTheme").val());
  $("#previewPageThemeButton").prop("href", $newPreviewUrl);
});
$("#previewPageThemeButton").click(function () {
  window.open($(this).prop("href"), "preview", "width=810,height=500,menubar=0,resizable=1,status=0,toolbar=0,scrollbars=1");
  return false;
});
```

However, I could not find the functionality that was used to produce the theme `select#previewPageTheme` and all the theme options. I assume this was handled on the server-side, possibly in the php code.

**Question:** Is there a function that will allow us to build a list of the themes and their IDs?

-------

Interestingly, there were some CSS rules in [page-edit.scss](https://riosalado.coursearc.com/css/src/page-edit.scss) but not in the versioned, non-scss [page-edit.css?version=…](https://riosalado.coursearc.com/css/page-edit.css?version=8954b24f10b1a397731d36b7480636d37e3139b3) file. 

```
/* Sidebar - preview */
#cms_preview_block select { margin-bottom: 10px;   }
#cms_preview_block .btn { margin: 0 auto; float: none; width: 50%; text-align: center; }
```
