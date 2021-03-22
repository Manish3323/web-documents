# The basics of snowpack & its advantage

If you are working on javascript/typescript based project which involves modern features of browsers, then knowing about Snowpack & its capability is a must. we will now see how it enables faster-development cycle in any web based application/library.

## Why using bundler's like webpack/parcel in development time is unnecessary ? 

For a starter, Let's say you are working on web application (it can be anything framework/library). In your development flow, you will most likely be touching not more than 2-3 files at a time and to compile these files and bundle the whole application only because of these changes in development time is unneccessary. you would be interested in only the current changes that you have made and compiling/transpiling(in case of typescript setup) or any type of transformation is only needed for the modified files(2) not for whole application.

There are features like incremental-compilation in webpack, which will only consider the modified files to apply it's compile/transformation step and apply the output(delta) to the bundler to create the bundle and then this bundle is served again & again for changes that you will be making the code base.
This step of bundling & serving is unneccessary because modern browsers are capable of understanding javascript in [[ESM]] format. 
So as long as your target audience is using browser's which supports ESM. you can make use of snowpack right away!! and you should definately consider over webpack.

Visual comparison of snowpack & webpack dev flow.
![[snowpack-unbundled-example-3.png]]
 `img-source: https://www.snowpack.dev/concepts/how-snowpack-works`

## Snowpack consist of ? 
- A fast dev server
- A extensible & easy to plugin system 
- A app generator CLI `create-snowpack-app(csa)` 
- Templates for various libraries like react/vue/svelte, etc. (some what similar to create-react-app) to be used along with CSA
- Snowpack CLI

## Features
- Instant or very fast startup time for the dev server. (50ms!!)
- Build any file once & cache it forever unless it is modified.
- Out of the box support for Typescript, JSX, CSS, CSS-modules,WASM, images & assets like font file, in the dev-server.
- Skypack CDN (Available from Snowpack version 3) : A kind of ESM Package manager, dedicated cdn  only for web packages.(could also be called as `non-node` module package manager).
- Production build support (using plugin system)(have this step only if you need it!!)
- Fast-refresh & esm-hmr (Hot module replacement on top of already fast dev server)
- Diverse set of templates available for popular libraries/frameworks
- Plugin system made easy. ( Learning curve not as steep as learning webpack's configs)


## List of all things snowpack uses behind the scenes to give these features.
- [[esbuild]]
- [[rollup]]
- [[chokidar]]
- [[esm-hmr]]

## Usecases when to change default snowpack config

	### TODO 

### Configuration file
Snowpack will look for a file named `snowpack.config.[js/json]` in root of your code base which has the config that mentions the overriden values of various config properties. 

List of top level config properties are : 
- root
- mount
- extends
- install
- alias
- plugin
- devOptions

## Let's create a new snowpack plugin
	### Todo