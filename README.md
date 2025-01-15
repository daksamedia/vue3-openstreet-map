
  

# vue3-openstreet-map


A Simple & free Vue 3 Openlayers Map Picker & Geocoder (Openstreet Map based) plugin



  

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
npm  i  vue3-openstreet-map@latest  --save
```

  

  

## How to use / Usage

  

  

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

  

4. Insert inside your component

  

```
// put this in template area & add width and height of parent div
<div style="width: 60%; position: relative; height: 600px;">
	<VueOpenMap :options="options" @on-change="onMyMapChange" />
</div>
```

  

  

## Options

**Positions**

position (object) - Coordinate / position options

- lat (integer) - Latitude number

- lon (integer) - Longitude number

- zoom (integer) - Map zoom size

  

**Search**

search (object) - Search input options

- show (boolean) - Show / hide search input

- placeholder (string) - Placeholder text for search input

- language (string) - Language | default : 'en' | e.g: ‘en’, ‘id’, ‘jp’, etc

  

**Picker**

picker (object) - Marker options

  

- show (boolean) - Show / hide popup info

- label (string) - Popup info label

- icon (string) - Icon image file

  
  

## Event

-  `onChange` - Triggered when map is clicked marker is moved
	- response
		- `address_name` - Local address name
		- `address_details` - Address details
		- `origin` - Original coordinates
			- `lat` : Original latitude
			- `lon` : Original longitude
		- `coordinates`- Native coordinates
			- `lat` : Native latitude
			- `lon` : Native longitude
	- example usage
		
    ```
    onMyMapChange(event) => {
      // response will be getting on event
      console.log(event)
   }
   ```
## Notes

  

From 🇮🇩 with 💗

Developed by [Daksa Media](https://daksamedia.id)

  

  

## License
MIT


**Free Software, Hell Yeah!**
