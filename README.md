# ⚔️ SidePeek.js — Offensive Browser Recon Toolkit

> **Client-Side Payloads for Red Teamers, Bug Bounty Hunters, and OSINT Explorers**

**SidePeek.js** is a curated, open-source collection of JavaScript payloads you can run *directly* in the browser's Developer Console or as bookmarklets. Designed for **offensive recon**, **DOM-based analysis**, and **client-side attack surface discovery**, these payloads empower Red Teamers and hackers to see what lies beneath the surface.

## 🧠 Why SidePeek?

Modern frontends bury critical attack surfaces in layers of JavaScript and dynamic DOM logic. SidePeek gives red teamers, bug hunters, and curious hackers the edge to peel that back. These payloads reveal:

- 🔍 Hidden API calls, dynamic routes, and credentials
- 🛠️ Embedded secrets and inline JS tokens
- 💣 DOM XSS sinks, client-side handlers, and shadow injections
- 🧪 CSRF-prone dynamic forms and tampered inputs
- 🧬 JavaScript file mappings, local storage values, iframe sources
- ⚡ Event-based execution flows and inline triggers
- ...and a lot more that devs didn’t expect you to see

> No dependencies. No noise. Just raw, client-side recon at your fingertips.

## 🧪 Quick Start

Open your browser’s DevTools Console, then simply copy and paste the payload — and run it. That’s it.

### Extract All URLs From the Page
```javascript
(function(){var s=document.getElementsByTagName('*'),r=/https?:\/\/[^\s"'<>]+/g,matches=new Set();for(let i=0;i<s.length;i++){let el=s[i].outerHTML.match(r);if(el)el.forEach(url=>matches.add(url))}document.write(Array.from(matches).join('<br>'));})();
```
> Gather all URLs present on the page. This is useful for finding all external resources like images, CSS files, API endpoints, and links.

### Find Inline JS Secrets / Tokens
```javascript
(function(){let scripts=document.querySelectorAll('script:not([src])');let regex=/([A-Z0-9_]{8,}) ?[:=] ?[\'\"\`](.*?)['\"\`]/gi;let found=new Set();scripts.forEach(s=>{let matches=s.innerText.matchAll(regex);for(const m of matches)found.add(m[0]);});document.write([...found].join('<br>'));})();
````
> Detect inline JavaScript secrets or tokens (such as API keys, authentication tokens, or session identifiers). This can be helpful in reviewing the page for potentially exposed sensitive data

### Scan for DOM XSS Sinks
```javascript
(function(){var sinks=['innerHTML','outerHTML','document.write','eval','setTimeout','setInterval'];var found=[];document.querySelectorAll('script').forEach(s=>{sinks.forEach(k=>{if(s.innerText.includes(k))found.push(k)})});alert("Potential Sinks:\n"+[...new Set(found)].join("\n"));})();
````
> Identify potential sinks in the DOM that could lead to Cross-Site Scripting (XSS) vulnerabilities. This scan helps to find locations where untrusted data could be injected into the page's DOM.

### Dump JS Files Used by Page
```javascript
(function(){var urls=[];document.querySelectorAll("script[src]").forEach(s=>{urls.push(s.src)});document.write(urls.join("<br>"));})();
(…and many more inside the /payloads folder)
````
> **Use Case** : List all external JavaScript files loaded by the page. This is helpful for understanding the external resources and libraries used, which could potentially introduce vulnerabilities or require further analysis.


### Dump All pathway From JS files
```js
javascript:(function(){var scripts=document.getElementsByTagName("script"),regex=/(?<=(\"|\'|\`))\/[a-zA-Z0-9_?&=\/\-\#\.]*(?=(\"|\'|\`))/g;const results=new Set;for(var i=0;i<scripts.length;i++){var t=scripts[i].src;""!=t&&fetch(t).then(function(t){return t.text()}).then(function(t){var e=t.matchAll(regex);for(let r of e)results.add(r[0])}).catch(function(t){console.log("An error occurred: ",t)})}var pageContent=document.documentElement.outerHTML,matches=pageContent.matchAll(regex);for(const match of matches)results.add(match[0]);function writeResults(){results.forEach(function(t){document.write(t+"<br>")})}setTimeout(writeResults,3e3);})();
````
> Use Case** : Extracts all relative paths or routes from JavaScript files loaded by the page. It's useful for analyzing the internal structure of a web application and identifying endpoints or resources that may require further investigation for vulnerabilities.
### Reveal Hidden Input Fields
```js
javascript:(function(){document.querySelectorAll('input[type="hidden"]').forEach(el => { el.type = 'text'; el.style.border = '2px solid red'; });})();
```
> Use Case: Uncover hidden form fields that might store sensitive data or tokens.

### Extract All Forms and Their Actions
````js
javascript:(function(){var forms=document.forms;var info='';for(var i=0;i<forms.length;i++){info+='Form '+(i+1)+': '+forms[i].action+'\n';}alert(info);})();
````
> **Use Case** : Enumerate all forms and their submission endpoints.

### Check for Inline Event Handlers
````js
javascript:(function(){var elements=document.querySelectorAll('[onclick],[onmouseover],[onload]');alert('Elements with inline event handlers: '+elements.length);})();
````
> **Use Case** : Detect potential XSS vectors via inline event handlers


### List All Iframes
````js
javascript:(function(){var iframes=document.getElementsByTagName('iframe');var srcs=[];for(var i=0;i<iframes.length;i++){srcs.push(iframes[i].src);}alert(srcs.join('\n'));})();
````
> **Use Case** : Identify embedded iframes which might be used for clickjacking or loading external content.


### Enumerate Local Storage

````js
javascript:(function(){var ls='';for(var i=0;i<localStorage.length;i++){ls+=localStorage.key(i)+': '+localStorage.getItem(localStorage.key(i))+'\n';}alert(ls);})();
````
> **Use Case** : Inspect data stored in the browser's local storage. 

### Extract All Email Addresses
````js
javascript:(function(){var body=document.body.innerHTML;var emails=body.match(/[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-z]{2,}/g);alert(emails?emails.join('\n'):'No emails found.');})();
````
> **Use Case** : Harvest email addresses from the current page.

🤝 Contribute to SidePeek.js
SidePeek.js is open source because I believe in offensive knowledge sharing. These payloads have helped me find low-hanging fruit and high-value intel during real-world bug bounty work. From hidden APIs to exposed tokens — they’ve surfaced data I would’ve otherwise missed.

If you’ve got JavaScript payloads that can help others in Red Team ops, bug bounty, or client-side recon, contributions are welcome.

### 📜 Contribution Guidelines:
- No Malware or Illegal Code: Ensure all contributions are ethical and adhere to legal standards.
- Readable & Focused Payloads: Keep your payloads clean, concise, and to the point.
- Native JavaScript Only: Stick to pure JavaScript—no external libraries or dependencies.
- Credit for Contributions: All contributors will be properly credited for their work.
