# entropyCollector
Embeddable web widget that collects user entropy using mouse movements. Further this widget provides the generated random string/seed as a direct clipboard copy.


## Features
- Neat look!
- No dependencies
- Vanilla html/js injection
- Direct, private and "quasi-true"-random numbers
- Seed is an 8 bit number sequence

## Implementation
Copy the script below and paste it within the tags of the desired container (parent). For customizations tune the preliminary widget parameters:


```html
<!-- Embeddable Widget -->
<script>
    // ------ WIDGET PARAMETERS ------
    const gridLength = 20;
    const backgroundColor = '#0d0d0d';
    const hoverColors = ['#e6e600', '#ffff80', '#ffe066'];
    const fontColor = '#fff';
    const decayTime = 3 // seconds
    const radius = 15 // percent
    const menuHeight = 10 // percent
    const glowRadius = 10 // px
    // --------------------------------
    function u(){return Math.random()}function delay(seconds){return new Promise(function(resolve){setTimeout(function(){resolve(0)}, 1000*seconds)});}function get(i,j){return document.getElementById(`(${i}:${j})`)}
    function randomHoverColor(){if(hoverColors.length>1){return hoverColors[Math.round(u()*(hoverColors.length-1))]}else{return hoverColors[0]}}async function highlight(elem){try{elem.style.transition = "none";col = 
    randomHoverColor();elem.style.backgroundColor=col;elem.style.opacity="1.0";elem.style.boxShadow=`0 0 ${glowRadius}px ${Math.round(glowRadius/10)}px `+col;await delay(0.2*decayTime);}catch(e){/**/}}async function 
    off(elem){try{elem.style.transition=`${decayTime}s ease-out`;elem.style.opacity="0.0"}catch(e){/**/}}async function blink(elem,length){await highlight(elem);await delay(length);await off(elem)}async function blr 
    (length){try{var x=Math.round(u()*(gridLength-1));var y=Math.round(u()*(Math.round(gridLength-1)*(1-menuHeight/100)));blink(get(x,y),length);}catch(e){/**/}}async function bringToLife(){while(true){await delay(1
    /2*u());if(!hoverStatus){for(let i=0;i<Math.round(10*u());i++){blr();await delay(0.05*u())}}}}function refshSize(){if(entropy.length>1000){document.getElementById('b').innerHTML=`${entropy.length/1000} kB`} else
    {document.getElementById('b').innerHTML=`${entropy.length} Bytes`}}function resetSize(){entropy.length=0;document.getElementById('b').innerHTML=`0 Bytes`;}function copyPayLoad(){var momentum,ranAscii,uniqueValue
    ,totalDistance;var out=[];for(let i=0;i<entropy.length;i++){const pair=entropy[i];if(i==0){momentum=1}else{momentum=(entropy[i][0]-entropy[i-1][0])**2+(entropy[i][1]-entropy[i-1][1])**2}uniqueValue=((entropy[i][
    0]*gridLength+entropy[i][1]+1));totalDistance=(entropy[i][0]**2+entropy[i][0]**2);ranAscii=(momentum*uniqueValue*totalDistance)%255;out.push(ranAscii.toString())}out = out.join(' ');navigator.clipboard.writeText
    (out).then((async()=>{document.getElementById('c').innerHTML=`copied!`;await delay(3);document.getElementById('c').innerHTML=`copy`}));}var hoverStatus=false;var entropy=[];var parentNode=document.currentScript.
    parentNode;var container=document.createElement("div");container.style.position="relative";container.style.width="100%";container.style.height="100.%";var main=document.createElement("div");main.style.background
    =backgroundColor;main.style.position="relative";main.style.width="100.0%";main.style.height=`${100-menuHeight}%`;main.style.overflow="hidden";main.style.padding="0.0";parentNode.appendChild(container);container.
    appendChild(main);var grid=document.createElement("span");grid.style.width="100.0%";grid.style.height="100.0%";grid.style.display="inline-block";grid.style.fontSize="0.0";main.appendChild(grid);var menu=document
    .createElement("div");container.appendChild(menu);menu.style.width="100.0%";menu.style.height=`${menuHeight}%`;menu.style.backgroundColor=backgroundColor;["b","r","c"].forEach(elem=>{var element;element=document
    .createElement("button");element.style.textDecoration="none";element.style.border="none";element.style.position="relative";element.style.height="100%";element.style.backgroundColor=backgroundColor;element.style.
    width="33.33%";element.style.color=fontColor;element.style.display="inline-block";element.id=elem;if(elem=="r"){element.innerHTML="reset";element.onclick=resetSize;}else if(elem=="c"){element.innerHTML="copy"/**
    */element.onclick=copyPayLoad}else if(elem=="b"){element.innerHTML="0 Bytes";element.disabled=true}element.onmouseover=function(event){this.style.color=randomHoverColor()};element.onmouseout=function(event){this
    .style.color=fontColor};menu.appendChild(element)});var cell,col;const cellScale=`${parseFloat(parentNode.style.width)/gridLength}px`;for(let i=0;i<Math.round(gridLength*(100-menuHeight)/100);i++){for (let j=0;j
    <gridLength;j++){cell=document.createElement("span");cell.id=`(${i}:${j})`;cell.style.display="inline-block";cell.style.height=cellScale;cell.style.width=cellScale;cell.style.borderRadius=`${radius}% ${radius}% 
    ${radius}% ${radius}%`;cell.onmouseover=function(event){hoverStatus=true;highlight(this);entropy.push([i,j]);refshSize()};cell.onmouseout=function(event){hoverStatus=false;off(this)};grid.appendChild(cell)}}/**/
    bringToLife();parentNode.removeChild(parentNode.children[0]);/*BUFFER ########################################################################################################### https://github.com/B0-B © 2020 */
</script>
<!----------------------->
```

The script will render the widget according to it's parent geometry - make sure that the parent's width and height match to get a square shape. Your final web page might look like this:

```html
<!DOCTYPE html>
<html style="background-color: #383838;">
    <head>
    </head>
    <body>
        <div id="parent" style="height: 400px; width: 400px;">
            <!-- Embeddable Widget -->
            <script>
                ...
            </script>
            <!----------------------->
        </div>
    </body>
</html>
```