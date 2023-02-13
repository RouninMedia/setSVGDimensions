# setSVGDimensions
`setSVGDimensions()` is a javascript function which intelligently applies `width` and `height` attributes to dynamically-resizable **SVG**s.

This is useful, not least, when drawing dynamically-resizable **SVG**s to **HTML5 Canvas**.

If the **SVG** in question lacks a `viewBox` attribute, the function will apply to the **SVG**:

 - `width="200"`
 - `height="200"`
 
 If the **SVG** in question has a `viewBox` attribute but neither a `width` nor a `height` attribute, the function will apply to the **SVG**:
 
  - `width` derived from `viewBox`
  - `height` derived from `viewBox`
  
  
 If the **SVG** in question has a `viewBox` attribute and a `width` attribute (but no `height` attribute), the function will apply to the **SVG**:
 
  - `height` derived from `width` multiplied by height/width ratio of `viewBox`
  
  
 If the **SVG** in question has a `viewBox` attribute and a `height` attribute (but no `width` attribute), the function will apply to the **SVG**:
 
  - `width` derived from `height` divided by height/width ratio of `viewBox`
  
 _________

## `setSVGDimensions()` Function

```js
const setSVGDimensions = (vectorGraphic) => {

  let viewBoxValues = (vectorGraphic.getAttribute('viewBox')) ? vectorGraphic.getAttribute('viewBox').replace(/\s+/g, ' ').trim().split(' ') : ['0', '0', '200', '200'];
  let viewBoxWidth = viewBoxValues[2];
  let viewBoxHeight = viewBoxValues[3];
  let ratioHeightWidth = viewBoxHeight / viewBoxWidth;

  switch (true) {
  
    case ((vectorGraphic.getAttribute('width') === null) && (vectorGraphic.getAttribute('height') === null)) :
      vectorGraphic.setAttribute('width', viewBoxWidth);
      vectorGraphic.setAttribute('height', viewBoxHeight);
      break;
      
    case (vectorGraphic.getAttribute('width') === null) :
      let svgHeight = vectorGraphic.getAttribute('height');
      vectorGraphic.setAttribute('width', (svgHeight / ratioHeightWidth));
      break;
      
    case (vectorGraphic.getAttribute('height') === null) :
      let svgWidth = vectorGraphic.getAttribute('width');
      vectorGraphic.setAttribute('height', (svgWidth * ratioHeightWidth));
      break;
  }

  return vectorGraphic;
}
```

