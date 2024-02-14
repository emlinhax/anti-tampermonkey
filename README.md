# anti-monkey

prevent tampermonkey scripts from running on your website / online game. \
this also works against tampermonkey's "instant injection" mode :)

this code has to be placed into the DOM content. \
just add a script tag to your website and put this script in there. \
i recommend putting it above everything else due to execution order. \
</br>
how it works:
whenever tampermonkey injects a script, it has to run that script somehow. \
turns out that it has to modify the "document" root for that ([reference](https://github.com/Tampermonkey/tampermonkey/blob/07f668cd1cabb2939220045839dec4d95d2db0c8/src/environment.js#L52)) \
</br>
to catch this, we just register a mutation observer and wait for a script to be injected! \
</br>
NOTE: this snippet should be put into a script tag with the "defer" attribute

```js
const observer = new MutationObserver((mutations) => {
    mutations.forEach((mutation) => {
        if(mutation.addedNodes != null && mutation.addedNodes.length > 0)
        {
            for(var addedNode of mutation.addedNodes)
            {
                if(addedNode.innerHTML.includes("==UserScript=="))
                {
                    alert("cheater detected!")
                }
            }
        }
        console.log(mutation)
    });
});

observer.observe(document, { childList: true, subtree: true });
```
