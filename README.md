# svelte-json-tree-view

JSONTreeView component and action for [Svelte 3](https://svelte.dev). [demo](https://svelte.dev/repl/e59ce5db23594e0c814b7faa53c4bca3?version=3.29.4)

## Usage

Install with npm or yarn:

```bash
npm install --save-dev svelte-json-tree-view
```

## Parameters

Any options of `json-tree-view` can be passed to action params or TreeView props. All `json-tree-view` events are also available on component instance or DOM element which has action assigned.

```svelte
<label>Filter object:</label>
<input bind:value={filterText}>
<label>
  <input bind:checked={readonly} type="checkbox"> Readonly 
</label>

<TreeView {value} {readonly} {filterText} on:change={change} />

<script>
  import TreeView from 'svelte-json-tree-view';

  let value = { foo: 1, bar: true, baz: 'quux' };
  let readonly = false;
  let filterText = '';

  function change({ detail: { self, key, changes: [ oldValue, newValue ] } }) {
    console.log('changed', key, oldValue, newValue);
  }
</script>
```

OR import `treeview` action to get more control.

```html
<div 
  use:treeview={{ value }} 
  on:change={change}
/>

<textarea bind:value></textarea>
 
<script>
  import { treeview } from 'svelte-json-tree-view';

  let text = "";

  $: value = JSON.parse(text);

  function change({ detail: { self, key, changes: [ oldValue, newValue ] } }) {
    console.log('changed', key, oldValue, newValue);
  }
</script>
```

## TODO...

Readme is still a work-in-progress.

## License

MIT &copy; [PaulMaly](https://github.com/PaulMaly)
