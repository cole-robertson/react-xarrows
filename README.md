# react-xarrows

## introduction

Draw arrows between components in React!

This library is all about customizable and relaible arrows(or lines) between DOM elements in React.

I've needed such a components in one of my projects - and found out(surprisingly enough) that there is no such(good) lib, so I've decided to create one from scrate, and share it.

now (since v1.1.4) i can say, after a lot of tests and improvements- the arrows should work and act naturally under any normal circumstances(but don't try put some negative curveness ha?).

found a problem? not a problem! post a new issue([here](https://github.com/Eliav2/react-xarrows/issues)) and i will do my best to fix it.

liked my work? please star this repo.

this project developed [using codesandbox](https://codesandbox.io/s/github/Eliav2/react-xarrows).

#### what to expect

- this arrows will rerender and will update the arrows position whenever needed(not like other similar npm libraries).
- works no matter where the Xarrow component placed in the DOM relative to his anchors.
- works no matter what the type of elemnts the anchors are (like div,p, h1, and so on).
- nice intellij suggestinons will apear when working with Xarrow.
- works inside scrollable windows(no matter how many - or even if any anchor element inside diffrent nested scrolling windows).
- you can give this component simple props or more detailed ones for more custom behavior and looking.
- please see the examples below to understand better the using and features.

## installation

with npm `npm install react-xarrows`.
(or `yarn add react-xarrows`)

## Examples

[see here!](https://codesandbox.io/embed/github/Eliav2/react-xarrows/tree/master/examples?fontsize=14&hidenavigation=1&theme=dark) codebox of few examples(in this repo at /examples).

![Image of xarrows](https://github.com/Eliav2/react-xarrows/blob/master/examples/images/react-xarrow-picture-1.2.0.png)

### simple example:

```jsx
import React, { useRef } from "react";
import Xarrow from "react-xarrows";

const boxStyle = {
  border: "grey solid 2px",
  borderRadius: "10px",
  padding: "5px",
};

function SimpleExample() {
  const box1Ref = useRef(null);
  return (
    <div style={{ display: "flex", justifyContent: "space-evenly", width: "100%" }}>
      <div ref={box1Ref} style={boxStyle}>
        hey
      </div>
      <p id="elem2" style={boxStyle}>
        hey2
      </p>
      <Xarrow
        start={box1Ref} //can be react ref
        end="elem2" //or an id
      />
    </div>
  );
}

export default SimpleExample;
```

## The API

see 'Example2' at the examples codesandbox to play around.

the properties the xarrow component recieves is as follow(as listed in `xarrowPropsType` in /lib/xarrow.d.ts):

```jsx
export type xarrowPropsType = {
  start: refType;
  end: refType;
  startAnchor?: anchorType | anchorType[];
  endAnchor?: anchorType | anchorType[];
  label?: labelType | { start?: labelType; middle?: labelType; end?: labelType };
  color?: string;
  lineColor?: string | null;
  headColor?: string | null;
  strokeWidth?: number;
  headSize?: number;
  curveness?: number;
  dashness?: boolean | { strokeLen?: number; nonStrokeLen?: number; animation?: boolean | number };
  monitorDOMchanges?: boolean;
  registerEvents?: registerEventsType[];
  consoleWarning?: boolean;
  advanced?: {
    extendSVGcanvas?: number;
  };
};

export type anchorType = anchorMethodType | anchorPositionType;
export type anchorMethodType = "auto";
export type anchorPositionType = "middle" | "left" | "right" | "top" | "bottom";
export type reactRefType = { current: null | HTMLElement };
export type refType = reactRefType | string;
export type labelType = string | { text: string; extra?: SVGProps<SVGElement> };
export type domEventType = keyof GlobalEventHandlersEventMap;
export type registerEventsType = {
  ref: refType;
  eventName: domEventType;
  callback?: CallableFunction;
};
```

you can keep things simple or provide more detailed props - the API except both.
for example - you can provide `label:"middleLable"` and the string will apear as middle label or customize the labels as you please: `label:{end:{text:"end",extra:{fill:"red",dx:-10}}}`.
see typescript types above for detailed descriptions of what type excepts every prop.

#### 'start' and 'end'

can be a reference to a react ref to html element or string - an id of a DOM element.

#### 'startAnchor' and 'endAnchor'

each anchor can be: `"auto" | "middle" | "left" | "right" | "top" | "bottom"`.
`auto` will choose automatically the path with the smallest length.
can also be a list of possible anchors. if list is provided - the minimal length anchors will be choosed from the list.

#### label

can be a string that will default to be at the middle or an object that decribes where to place label and how to customize it. see `label` at `xarrowPropsType` above.
examples:

- `label="middleLabel"`
- `label={{ start:"I'm start label",middle: "middleLable",end:{text:"i'm custom end label",extra:{fill:"red"}} }}`
  you can pass to `extra` attributes of [svg text element](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/text).

#### color,lineColor and headColor

color defines color for all the arrow include head. if lineColor or headColor is given so it overides color specificaly for line or head.
examples:

- `headColor="red"` will change the color of the head of the arrow to red.

#### strokeWidth and headSize

strokeWidth defines the thickness of the arrow(line and head).
headSize defines how big will be the head relative to the line(set headSize to 0 to make the arrow like a line).

#### curvness

defines how much the lines curve.
0 mean stright line and 1 'perfect' curve. larger then 1 will be over curved.

#### dashness

can make the arrow dashed and can even animate.
if true default values(for dashness) are choosed. if object is passed then default values are choosed execpt what passed.
examples:

- `dashness={true}` will make the line of the arrow to be dashed.
- `dashness={{ strokeLen: 10, nonStrokeLen: 15, animation: -2 }}` will make a custom looking dashness

#### monitorDOMchanges

A boolean. set this property to true to add relevant eventListeners to the DOM so the xarrow component will update anchors position whenever needed(scroll and resize and so on).

#### registerEvents

you can register the xarrow to DOM event as you please. each time a event that his registed will fire the xarrow component will update his position and will call `callback` (if provided).

#### consoleWarning

we provide some nice warnings (and errors) whenever we detect issues. see 'Example3' at the examples codesandbox.

#### advanced

here i will provide some flexibility to the API for some cases that i may not thought of.
extendSVGcanvas will extend the svg canvas at all sides. can be usefull if for some reason the arrow(or labels) is cutted though to small svg canvas(should be used in advanced custom arrows, for example if you used `dx` to move one of the labels and at exceeded the canvas).

### default props

default props is as folows:

```jsx
Xarrow.defaultProps = {
  startAnchor: "auto",
  endAnchor: "auto",
  label: null,
  color: "CornflowerBlue",
  lineColor: null,
  headColor: null,
  strokeWidth: 4,
  headSize: 6,
  curveness: 0.8,
  dashness: false,
  monitorDOMchanges: true,
  registerEvents: [],
  consoleWarning: true,
  advanced: { extendSVGcanvas: 0 },
};
```

## Versions

- 1.0.0 - initial release.
- 1.0.3 - props added: `label`, `dashness` and `advance`.
- 1.1.0 - API changed! `arrowStyle` removed and all his contained properties flattened to be props of xarrow directly. `strokeColor` renamed to `lineColor`. `advance` renamed to `advanced`.
- 1.1.1 - bug fix now labels not exceed the svg canvas. the headArrow is calcualted now . this means the line ends at the start at the arrow - and this is more natural looking(especially at large headarrows).
- 1.1.2 bug fix. (the first arrow fixed the headarrow style for all next comming arrows)
- 1.1.3 - An entirely new algorithm to calcualte arrow path and curveness. now the arrow acting "smarter". this include bug fixes,improvements and some adjustments.
  `monitorDOMchanges` prop default changed to `true`.
- 1.1.4 - bug fixes, calculation optimizations, and smart svg canvas size adjusment.
- 1.1.5 - optimazed calculations and label positioning. (Buzier curve extrema point are calculated now using derivatives and not by interpolation) other improvements as well.
- 1.1.6 - errors and warnings improved. smart adjustments for diffrent positioning style(of anchors elemntes and common ancestor element) . minor bug fixes.
- 1.1.7 minor bug fixes.
- 1.2.0 added support for javascript projects that imported the lib locally. many changes to the repo folders structure.
- 1.2.1 minor bug fix (intellij suggestinons did not apear)
