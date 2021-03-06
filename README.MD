# react-omit
**tl;dr:** This package lets you omit the second argument (props) when using `React.createElement()` instead of JSX.

JSX (and XML in general) looks messy due closing tags and the attributes all being on one line. You can use React without JSX but `React.createElement()` hasn't been made very user-friendly.

After trying `hyperscript`, `r-dom`, and even trying to implement `pug`, I concluded that a shorthand for `React.createElement` is indeed the simplest, but that when no attributes are passed you're forced to put an empty object as the second argument. This little function removes that need.

As a bonus, combined with the new Coffeescript 2, you can omit the closing tags altogether.
The result is the same syntax for all JS and JSX in a file, with easy to type code and clearer oversight.

It allows code like this:
```es6
import React from 'react'
import o from 'react-omit'
import { Grid, Row, Col } from 'react-flexbox-grid'

const Home = () =>
  o(Grid,
    o(Row,
      o(Col, {xs: 6},
        o('h2', 'All Users'),
        o('ul', props.users.map((user, key) =>
          o('li', {key: key},
            o('b', 'Name: '),
            user.name,
            o('br'),
            o('b', 'Email: '),
            user.email
          )
        ))
      ),
      o(Col, {xs: 6},
        o('h2', 'All Projects'),
        o('ul', props.projects.map((project, key) =>
          o('li', {key: key},
            o('b', 'Title: '),
            project.title,
            o('br'),
            o('b', 'Owner: '),
            project.owner
          )
        ))
      )
    )
  )

export default Home
```

Or, using Coffeescript 2, allows you to write without closing tags, which looks very much like HAML or jade / pug:
```coffee
import React from 'react'
import o from 'react-omit'
import { Grid, Row, Col } from 'react-flexbox-grid'

const Home = () ->
  o Grid,
    o Row,
      o Col, {xs: 6},
        o 'h2', 'All Users'
        o 'ul', props.users.map (user, key) ->
          o 'li', {key: key},
            o 'b', 'Name: '
            user.name
            o 'br'
            o 'b', 'Email: '
            user.email
      o Col, {xs: 6},
        o 'h2', 'All Projects'
        o 'ul', props.projects.map (project, key) ->
          o 'li', {key: key},
            o 'b', 'Title: '
            project.title
            o 'br'
            o 'b', 'Owner: '
            project.owner

export default Home

```

Even better, since Coffeescript allows for objects to look like YML, you can do this:
```coffee
import React from 'react'
import o from 'react-omit'

Loading = () ->
  o 'svg',
    width: 38
    height: 38
    viewBox: '0 0 38 38'
    xmlns: 'http://www.w3.org/2000/svg'
    stroke: '#00AA11'
    o 'g',
      fill: 'none'
      fillRule: 'evenodd'
      o 'g',
        transform: 'translate(1 1)'
        strokeWidth: 2
        o 'circle',
          strokeOpacity: '.5'
          cx: 18
          cy: 18
          r: 18
        o 'path',
          d: 'M36 18c0-9.94-8.06-18-18-18'
          o 'animateTransform',
            attributeName: 'transform'
            type: 'rotate'
            from: '0 18 18'
            to: '360 18 18'
            dur: '1s'
            repeatCount: 'indefinite'

export default Loading
```

To generate this:
```html
<svg width="38" height="38" viewBox="0 0 38 38" xmlns="http://www.w3.org/2000/svg" stroke="#00AA11">
  <g fill="none" fill-rule="evenodd">
    <g transform="translate(1 1)" stroke-width="2">
      <circle stroke-opacity=".5" cx="18" cy="18" r="18"></circle>
      <path d="M36 18c0-9.94-8.06-18-18-18" transform="rotate(187.894 18 18)">
        <animateTransform attributeName="transform" type="rotate" from="0 18 18" to="360 18 18" dur="1s" repeatCount="indefinite"></animateTransform>
      </path>
    </g>
  </g>
</svg>
```

This code is super naive and would love your feedback and PR's 🤠
