# vue3-openstreet-map
A Simple Vue 3 Openlayers Map Picker & Geocoder (Openstreet Map based)

## Features
- Free to use (OpenLayers Map based)
- Easy to use
- Drag and drop marker
- Search location
- Reverse geocode
- Adaptive


## Prequisite

- Vue 3 + Vite / Nuxt 3
- Openlayers ([Download here](https://www.npmjs.com/package/ol))
- Openlayers Geocoder ([Download here](https://www.npmjs.com/package/ol-geocoder))
- Openlayers Popup ([Download here](https://www.npmjs.com/package/ol-popup))

## Installation

vue3-openstreet-map requires [Node.js](https://nodejs.org/) v10+ to run.

Install the dependencies and devDependencies and start the server.

```sh
npm i vue3-openstreet-map@latest --save
```

## How to use

1. Import the component 
```
import { VueOpenMap } from "vue3-openstreet-map" 
```
2. Import the style
```
import "/node_modules/vue3-openstreet-map/dist/style.css"
```
3. Put to the 'components' object (for options API)
```
export default defineComponent({
    components: {
        VueOpenMap
    }
})
```
4. Insert inside the ```<template>```
```
<VueOpenMap :options="options" />
```

## Options - coming soon

| Key | Type | Description |
| ------ | ------ | ------ |
| position | object | { lat, lon, zoom } |
| position > lat | integer | Latitude number |
| position > lon | integer | Longitude number |
| position > zoom | integer | Map zoom size |

and many more....
## License

MIT

**Free Software, Hell Yeah!**

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)

   [dill]: <https://github.com/joemccann/dillinger>
   [git-repo-url]: <https://github.com/joemccann/dillinger.git>
   [john gruber]: <http://daringfireball.net>
   [df1]: <http://daringfireball.net/projects/markdown/>
   [markdown-it]: <https://github.com/markdown-it/markdown-it>
   [Ace Editor]: <http://ace.ajax.org>
   [node.js]: <http://nodejs.org>
   [Twitter Bootstrap]: <http://twitter.github.com/bootstrap/>
   [jQuery]: <http://jquery.com>
   [@tjholowaychuk]: <http://twitter.com/tjholowaychuk>
   [express]: <http://expressjs.com>
   [AngularJS]: <http://angularjs.org>
   [Gulp]: <http://gulpjs.com>

   [PlDb]: <https://github.com/joemccann/dillinger/tree/master/plugins/dropbox/README.md>
   [PlGh]: <https://github.com/joemccann/dillinger/tree/master/plugins/github/README.md>
   [PlGd]: <https://github.com/joemccann/dillinger/tree/master/plugins/googledrive/README.md>
   [PlOd]: <https://github.com/joemccann/dillinger/tree/master/plugins/onedrive/README.md>
   [PlMe]: <https://github.com/joemccann/dillinger/tree/master/plugins/medium/README.md>
   [PlGa]: <https://github.com/RahulHP/dillinger/blob/master/plugins/googleanalytics/README.md>
