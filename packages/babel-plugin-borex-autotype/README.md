# Borex

Borex is helper library for [redux](http://redux.js.org/).

Learn it by [Documentation](http://kastigar.github.io/borex/) (sorry, it is still draft in russian).

Also check rewritten [counter example](https://github.com/kastigar/borex/commit/07df6bcb780b733db3a8353205e8a5fd13108a9b) and another [remake](https://github.com/kastigar/borex/commit/5155cbe48f568709a65ea66b0e285f666956def7) with self-reducing actions.
## Action helper

```
npm install -S borex-actions
```

`borex-actions` provides utilities for action creating.

```js
import actionCreator from 'borex-actions/actionCreator';
import setPayload from 'borex-actions/setPayload';
import setMeta from 'borex-actions/setMeta';
import setType from 'borex-actions/setType';
import withReducerIn from 'borex-actions/withReducerIn';


export const increment = actionCreator();
export const decrement = actionCreator();

export const addItem = actionCreator(
  setPayload((id, text) => ({ id, text }))
);

export const fatAction = actionCreator(
  setType('Fat action'),
  setPayload((id, text) => ({ id, text })),
  setMeta('analytics', (id, text) => ({ event: 'fat-action', id, text })),
  withReducerIn('data.list', (state, action) => [...state, action.payload]),
);

```

## Reducer helpers

```
npm install -S borex-reducers
```

`borex-reducers` provides utilities for reducer declaration.

```js
import createReducer from 'borex-reducers/createReducer';
import createReducerIn from 'borex-reducers/createReducerIn';
import composeReducers from 'borex-reducers/composeReducers';
import appendIn from 'borex-reducers/appendIn';

import { increment, decrement, addItem } from './actions';

const counterReducer = createReducerIn('counter', (on) => {
  on(increment, counter => counter + 1);
  on(decrement, counter => counter - 1);
});

const dataReducer = createReducer((on) => {
  on(addItem, appendIn('data.list', data => { ...data, createdAt: Date.now() }));
});

const rootReducer = composeReducers(dataReducer, counterReducer);
```

## Autotype babel plugin

```
npm install -D babel-plugin-borex-autotype
```

`.babelrc`

```
{
  "plugins": [
    "babel-plugin-borex-autotype"
  ]
}
```

This plugin inserts names for all anonymous `actionCreator` calls.

**Input**(counter.js):

```js
const increment = actionCreator();
```

**Output**:

```js
const increment = actionCreator('counter/increment');
```

Check [plugin documentation page](https://kastigar.github.io/borex/docs/BabelAutotype.html) for more details.

## Name and logo

The idea is

`borex` = **BO**ilerplate **RE**ducer for redu**X**

Also it looks like Boreas with super-duper modern -X suffix :) This fact explains logo (Boreas is Greek god of North Wind).