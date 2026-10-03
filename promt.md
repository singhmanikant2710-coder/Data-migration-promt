[...document.querySelectorAll('img[src^="data:"]')].pop().parentElement.parentElement.outerHTML.replace(/base64,[^"]+/g, 'base64,...').slice(-800)
