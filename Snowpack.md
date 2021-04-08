# Snowpack

If you are working on javascript/typescript based project which involves modern features of browsers, then knowing about Snowpack & its capability is a plus.
In this post we will see how it enables faster-development cycle in any web based application/library compared to other traditional approaches.

## Problems with existing tools 
- Convulated setup involving various bundler's in development time
- Not making use of modern browser module system (es modules)

## Why using bundler's like webpack/parcel in development time is ? 

For a starter, Let's say you are working on web application (it can be anything framework/library). In your development flow, you will most likely be touching not more than 2-3 files at a time and to compile these files and bundle the whole application only because of these changes in development time is unneccessary. you would be interested in only the current changes that you have made and compiling/transpiling(in case of typescript setup) or any type of transformation is only needed for the modified files(2) not for whole application.

There are some features like incremental-compilation in webpack, which will only consider the modified files to apply it's compile/transformation step and apply the output(delta) to the bundler to create the bundle and then this bundle is served again & again for changes that you will be making in the code base.
This step of bundling & serving is unneccessary because modern browsers are capable of understanding javascript in [[ESM]] format. 
So as long as your target audience is using browser's which supports ESM. you can make use of snowpack right away!! and you should atleast consider once before using webpack for new projects.

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


## Important package's snowpack uses behind the scenes.
- [esbuild](https://github.com/evanw/esbuild)
- [rollup](https://rollupjs.org/guide/en/)
- [esinstall](https://github.com/snowpackjs/snowpack/tree/main/esinstall)


### Configuration file
Snowpack will look for a file named `snowpack.config.[js/json/mjs]` in root of your code base which has the config that mentions the overriden values of various config properties. 

List of some of top level config properties are : 
- root
- mount
- plugins
- devOptions
- buildOptions
- testOptions
- etc

we will make use of some of these properties in the next section.
Snowpack default are sensible in  most of the cases, in case if you have done something unique inside your application. All configuration properties can be found [here](https://www.snowpack.dev/reference/configuration)
## How to get started ?


## How to migrate to snowpack in existing web applications

For ex. if you have folder structure like this

![[folder-structure-initial.png]]

Only thing snowpack will need to know is that which folders it needs to serve in dev & build mode which can be configured using `mount` property.

Following are the only steps required for you get started with snowpack workflow.
1. you would want to mount `src` folder in snowpack config.
```json
{
	"mount": {
		"public": "/",
		"src": "dist"
	}
}
```

2. update the entrypoint in index.html
Entrypoint ideally would  be `src/index.js` file or any other file inside src from where the application code starts.

`<script src='/dist/index.js' type="module"></script>`

**Notice that the script is loaded as esmodule & is being served from dist folder & not src folder**

Next, run `snowpack dev` command in terminal.

once the localhost server is up & running, it automatically open browser with `localhost:8080/`, the request to snowpack dev server will identify the index file inside public folder.

From here onwards, all imports mentioned in the entrypoint js module & the transitive imports will be requested by browser to the snowpack server.
 
this much configuration is sufficient to get you started. ✅ 

### Essentials
1. How to support typescript in your application ?
	- Add tsconfig.json for tsc to work.
	- Add `@snowpack/plugin-typescript` plugin in snowpack.config.json.
	
	```json
	{
		"mount": {
				"public": "/",
				"src": "dist"
			},
		"plugins": [[@snowpack/plugin-typescript]]
	}
    ```
	
Do checkout plugins section in [snowpack](https://snowpack.dev) for other plugins.
2. How to enable bundling for deployment only?
	although snowpack recommends unbundled development, we may want to deploy minimal payload of our application & hence we will need some build time optimizations like (dead code elimination, minification, uglify files,  etc.)
	
	Enable bundling process by adding in a  bundler-plugin provided by snowpack team.
	snowpack is going to use popular & fast `esbuild` package when it gets mature.  
	this is experimental feature as i am writing this post.
	
	Another popular bundler is webpack,  which can be plugged into the build pipeline of snowpack at bundling phase by adding  a plugin `@snowpack/plugin-webpack.`
	Read more on  this in detail [here](https://www.snowpack.dev/concepts/build-pipeline).

## Some cool stuff which can done with snowpack + skypack combination
1. setting `packageOptions.source=remote` in snowpack.config.js tells snowpack to do few things in certain way.
	- load esm packages for dependencies used in application from skypack cdn instead of local node_modules folder.
	  for ex. `react`, `react-dom`, etc.
	- this minimizes one more step `npm install` in our development workflow.
	
How it works ?

For the very time, when snowpack encounters `absolute` import statements in js/ts files, it resolves those dependencies from skypack cdn once and keeps it in snowpack cache folder.
So next time, snowpack can work offline too.

Essentially only time u would do  npm install is while installing snowpack itself.
then onwards, if you need to add a package in your. application.
`snowpack add react` command adds it in snowpack.deps.json.






## List of other interesting packages provided by snowpack

- [skypack](https://github.com/snowpackjs/snowpack/tree/main/skypack)
- [esinstall](https://github.com/snowpackjs/snowpack/tree/main/esinstall)
- [create-snowpack-app](https://github.com/snowpackjs/snowpack/tree/main/create-snowpack-app)


####  Another viable alternative [vite](https://vite.js) for snowpack is also gaining popularity in the Modern web development tools section.

