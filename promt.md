document.querySelector('img[src^="data:"]').parentElement.outerHTML.replace(/base64,[^"]+/, 'base64,...').slice(0, 400)
