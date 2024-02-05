# Dark-Patterns-
# JSON CODE
{
  "manifest_version": 2,
  "name": "Dark Pattern 2.0",
  "version": "2.0",
  "description": "Detects dark patterns on websites",
  "permissions": ["activeTab", "storage"],
  "browser_action": {
    "default_icon": {
      "16": "icons/icon16.png",
      "48": "icons/icon48.png",
      "128": "icons/icon128.png"
    },
    "default_title": "Dark Pattern 2.0",
    "default_popup": "popup.html"
  },
  "icons": {
    "16": "icons/icon16.png",
    "48": "icons/icon48.png",
    "128": "icons/icon128.png"
  },
  "content_scripts": [
    {
      "matches": ["<all_urls>"],
      "js": ["content.js"]
    }
  ]
}



# HTML FOR POP-UP
<!DOCTYPE html>
<html>
  <head>
    <title>Dark Pattern 2.0</title>
    <style>
      body {
        width: 200px;
        text-align: center;
      }

      #result {
        font-weight: bold;
        margin-top: 10px;
      }
    </style>
  </head>
  <body>
    <h2>Dark Pattern 2.0</h2>
    <button id="detectButton">Start Detecting</button>
    <div id="result"></div>
    <script src="popup.js"></script>
  </body>
</html>


# JAVA SCRIPT CONTENT
function detectDarkPatterns() {
  const containsdetectDarkPatterns = document.body.innerText.includes("detectDarkPatterns");
  return containsdetectDarkPatterns;
}
chrome.runtime.onMessage.addListener((request, sender, sendResponse) => {
  if (request.action === "detectDarkPatterns") {
    const result = detectDarkPatterns();
    sendResponse({ result });
  }
});


# JAVA SCRIPT POP-UP
document.getElementById("detectButton").addEventListener("click", () => {
  chrome.tabs.query({ active: true, currentWindow: true }, (tabs) => {
    const activeTab = tabs[0];
    chrome.tabs.sendMessage(activeTab.id, { action: "detectDarkPatterns" }, (response) => {
      const resultElement = document.getElementById("result");
      if (response.result) {
        resultElement.textContent = "Dark pattern detected!!! WARNING";
      } else {
        resultElement.textContent = "No dark pattern.";
      }
    });
  });
});
