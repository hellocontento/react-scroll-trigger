# 📜 react-scroll-trigger

[![npm version](https://badge.fury.io/js/react-scroll-trigger.svg)](https://badge.fury.io/js/react-scroll-trigger)
[![npm](https://img.shields.io/npm/l/express.svg)](LICENSE)
[![Coverage Status](https://coveralls.io/repos/github/ryanhefner/react-scroll-trigger/badge.svg?branch=master)](https://coveralls.io/github/ryanhefner/react-scroll-trigger?branch=master)
[![CircleCI](https://circleci.com/gh/ryanhefner/react-scroll-trigger.svg?style=shield)](https://circleci.com/gh/ryanhefner/react-scroll-trigger)
[![Greenkeeper badge](https://badges.greenkeeper.io/ryanhefner/react-scroll-trigger.svg)](https://greenkeeper.io/)

React component that monitors `scroll` events to trigger callbacks when it enters,
exits and progresses through the viewport. All callback include the `progress` and
`velocity` of the scrolling, in the event you want to manipulate stuff based on
those values.

## Install

Via [npm](https://npmjs.com/package/react-scroll-trigger)

```sh
npm install react-scroll-trigger
```

Via [Yarn](http://yarn.fyi/react-scroll-trigger)

```sh
yarn add react-scroll-trigger
```

## How to use

```js
import ScrollTrigger from 'react-scroll-trigger';

...

  onEnterViewport() {
    this.setState({
      visible: true,
    });
  }

  onExitViewport() {
    this.setState({
      visible: false,
    });
  }

  render() {
    const {
      visible,
    } = this.state;

    return (
      <ScrollTrigger onEnter={this.onEnterViewport} onExit={this.onExitViewport}>
        <div className={`container ${visible ? 'container-animate' : ''}`}
      </ScrollTrigger>
    );
  }
```

The `ScrollTrigger` is intended to be used as a composable element, allowing you
to either use it standalone within a page (ie. no children).

```js
  <ScrollTrigger onEnter={this.onEnterViewport} onExit={this.onExitViewport} />
```

Or, pass in children to receive events and `progress` based on the dimensions of
those elements within the DOM.

```js
  <ScrollTrigger onEnter={this.onEnterViewport} onExit={this.onExitViewport}>
    <List>
      [...list items...]
    </List>
  </ScrollTrigger>
```

You can also use inside of a scrollable container.

```jsx
  /** 
   * Scrollable div container is needed
   * 
   * We need the 'ref' from this container div, since this component will be loaded before our 'ref',
   * is ready I find it easier to store the 'ref' in state. 
   */
  const [myRef, setMyRef] = useState();
  <div ref={r => setMyRef(r)} style={{ height: '300px', overflow: 'scroll'}}>
    {/** 
      * ScrollTrigger must use the 'ref' inside of the 'containerRef' prop!
      *  
      * Use an array of components. This allows you to trigger when a component
      * becomes visible within the scrollable div 
      */}
    {myObjects.map((obj, index) => {
      return (
        <ScrollTrigger                     
          containerRef={myRef} 
          onEnter={e => alert(`${obj.attribute} at index ${index} is now visible inside scrollable div!`)} 
          onExit={e => alert(`${obj.attribute} at index ${index} is no longer visible inside scrollable div!`)} 
          onProgress={e => alert(`${obj.attribute} Progress: ${e}`)} 
          key={`${obj.attribute}_${index}`} 
        >
          <SomeOtherComponent someProp={obj.attribute} />
        </ScrollTrigger>
      )
    })}
  </div>
```

The beauty of this component is its flexibility. I’ve used it to trigger
AJAX requests based on either the `onEnter` or `progress` of the component within
the viewport. Or, as a way to control animations or other transitions that you
would like to either trigger when visible in the viewport or based on the exact
`progress` of that element as it transitions through the viewport.

### Properties

Below are the properties that can be defined on a `<ScrollTrigger />` instance.
In addition to these properties, all other standard React properites like `className`,
`key`, etc. can be passed in as well and will be applied to the `<div>` that will
be rendered by the `ScrollTrigger`.

* `component:Element | String` - React component or HTMLElement to render as the wrapper for the `ScrollTrigger` (Default: `div`)
* `containerRef:Object | String` - DOM element instance or string to use for query selecting DOM element. (Default: `document.documentElement`)
* `throttleResize:Number` - Delay to throttle `resize` calls in milliseconds (Default: `100`)
* `throttleScroll:Number` - Delay to throttle `scroll` calls in milliseconds (Default: `100`)
* `triggerOnLoad:Boolean` - Whether or not to trigger the `onEnter` callback on mount (Default: `true`)
* `onEnter:Function` - Called when the component enters the viewport `({progress, velocity}, ref) => {}`
* `onExit:Function` - Called when the component exits the viewport `({progress, velocity}, ref) => {}`
* `onProgress:Function` - Called while the component progresses through the viewport `({progress, velocity}, ref) => {}`

## License

[MIT](LICENSE) © [Ryan Hefner](https://www.ryanhefner.com)
