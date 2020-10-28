<div
	use:treeview={{ caption, value, ...$$restProps }}
	on:change={change}
    on:change
    on:rename
    on:delete
    on:append
    on:click
    on:expand
    on:collapse
    on:refresh
/>

<script context="module">
	import JSONTreeView from 'json-tree-view';

	const events = [
		'change',
		'rename',
		'delete',
		'append',
		'click',
		'expand',
		'collapse',
		'refresh'
	];

	export function treeview(node, { caption, value, ...options }) {

		const view = new JSONTreeView(caption, value, null);

		let _expand, _collapse;

		function update({ caption, value, expand = true, collapse = true, ...options }) {
			if (_expand !== expand) {
				view.expand(expand);
				_expand = expand;
			}

			Object.keys(options).forEach(key => {
				view.hasOwnProperty(key) && (view[key] = options[key]);
			});

			if (view.value !== value) {
				view.value = value;
				view.refresh();
			}

			if (_collapse !== collapse) {
				view.collapse(collapse);
				_collapse = collapse;
			}
		}

		events.forEach(eventName => {
			view.on(eventName, (self, key, ...changes) => {
				fire(node, eventName, { self, key, changes });
			});
		});

		update({ caption, value, ...options });

		node.appendChild(view.dom);

		return {
			update,
			destroy() {
				view.destroy();
			}
		};
	}

	function fire(el, name, detail) {
		const e = new CustomEvent(name, { detail });
		el.dispatchEvent(e);
	}
</script>

<script>
	export let caption = 'JSON';
	export let value = '';

	function change({ detail }) {
		value = detail.self.value;
	}
</script>

<style global>
    @import 'json-tree-view/devtools.css';
    .custom-control-label:hover {
        cursor: pointer;
    }
</style>
