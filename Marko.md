
### key features provided
- is isomorphic. runs on
- Server side rendering of components (react is still in experimental phase)
- is a compiler (similar to svelte) but renders to V-DOM.
	- The Marko runtime is not distributed as a single JavaScript file. Instead, the Marko compiler generates a JavaScript module that will only import the parts of the runtime that are actually needed.	
	- Compile-time optimization of static sub-trees
- Tries to use native things provided by browsers as much as possible
	- 