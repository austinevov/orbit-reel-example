### Documentation

To initialize the library, call:

`orbitCreelCreateFromUrsula(container, token, onUnitClicked)`

##### container

a reference to the HTMLDivElement the library will inject into. The parent container must have an aspect ratio of 16:9 (in other words, the height must be 9/16 the width of the container). The library will fill to 100% of the width/height of the container.

##### token

This will be provided.

##### onUnitClicked

A callback that's called whenever a unit is clicked, either in the keyplan floor view, or on the main orbit reel 3D.

A unit has the following prototype:

```
{
  name: string;
  bedrooms: number;
  bathrooms: number;
  isUnavailable?: boolean;
}
```

### Getting Started

1. Create your container element. Must have a fixed dimension of 16:9.
2. Import the orbit-reel.js script
3. Invoke the library via orbitReelCreateFromUrsula:

```
<script>
  const root = document.getElementById('root');

  OrbitReel.orbitReelCreateFromUrsula(
	root,
	`${TOKEN}`,
	unit => {
	  //handle unit click
	}
  );
</script>
```

### Custom Styling

All selectors are prefixed with 'ev-'. Simply insert a separate stylesheet that defines rules for specific selectors, and your styling should cascade naturally. CSS was written to prevent strict coupling, so you shouldn't need to use any inelegant CSS hacks like `!important` and so forth.

### Object Documentation

```
IUnit {
	id?: number;
	name: string;
	bedrooms: number;
	bathrooms: number;
	isUnavailable?: boolean;
	floor: number;
  floorplanName: string;
  floorplanAssetsSrc: string[];
  price: number;
  squareFeet: number;
}
```

```
IKeyplan {
	floor: number;
	fileName: string; //path to actual keyplan image resource
}
```

```
IOrbitReelOptions {
	units?: IUnit[];
	onUnitClicked?: (unit: IUnit) => void;
	//Called when the orbit reel is rotated. Theta ranges from 0-360. Dividing by three will give the exact frame number.
	onThetaChanged?: (theta: number) => void;
}
```

```
ICustomOptions {
	disableUnitFilterControls?: boolean;
		DEFAULT: false
	mode?: 'all' | 'units' | 'amenities';
		DEFAULT: 'all'
	initialFrameNumber?: number;
		DEFAULT: 0
}
```

```
OrbitReelInstance {
	overrideFilter: (filter: (unit: IUnit) => boolean);
}
```

### API Documentation

If your property/development already uses Ursula as a backend dependency, then the library can automatically pull most building information from Ursula without requiring manual input, thus simplifying the API entry point:

```
orbitReelCreateFromUrsula(
	container: HTMLDivElement,
	token: string,
	onUnitClicked?: (unit: IUnit) => void,
	onThetaChanged?: (theta: number) => void,
	customOptions?: ICustomOptions
): Promise<OrbitReelInstance>
```

Example:

```
const root = document.getElementById('root');

OrbitReel.orbitReelCreateFromUrsula(
	root,
	'SOME_TOKEN_HERE',
	unit => {
		console.log(unit);
	}
);
```

Regular, non-Ursula entrypoint:

```
orbitReelFromToken(
  container: HTMLDivElement,
  token: string,
  externalOptions: IOrbitReelOptions,
	customOptions?: ICustomOptions
): Promise<OrbitReelInstance>
```

Example:

```
const root = document.getElementById('root');

OrbitReel.orbitReelFromToken(
	root,
	'SOME_TOKEN_HERE',
	{
		units: [
			{name: '107', bedrooms: 1, bathrooms: 1, isUnavailable: false, floor: 1},
			{name: '207', bedrooms: 1, bathrooms: 1, isUnavailable: false, floor: 2},
			{name: '307', bedrooms: 2, bathrooms: 2, isUnavailable: false, floor: 3},
			{name: '407', bedrooms: 4, bathrooms: 4, isUnavailable: false, floor: 4},
		],
		onUnitClicked: (unit) => {
			console.log(unit);
		},
		onThetaChanged: (theta) => {
			console.log(theta)
		}
	}
);
```

### Manipulating the Orbit Reel instance dynamically

Initializing the Orbit Reel (either through orbitReelFromToken or orbitReelCreateFromUrsula) returns an OrbitReelInstance promise. This object permits direct access to the Orbit Reel instance as it's running, and can be used to manipulate the control flow of the viewer.

See the object documentation above for a full list of manipulation methods available.

When used in conjunction with `disableUnitFilterControls`, this function interface can be used to radically overhaul the inner workings of the application. For instance, users can disable the built-in unit filter controls, and provide their own -- creating and passing the updated unit filter directly to the library.

Example showcasing a custom 'square foot' unit filter that hides units below the inputted square foot value:

```
<body>
    <div id="root"></div>
    <input type="number" id="minSquareFootFilter" />
    <script>
      const root = document.getElementById('root');
      const input = document.getElementById('minSquareFootFilter');

      OrbitReel.orbitReelCreateFromUrsula(
        root,
        'YOUR_TOKEN_HERE',
        unit => {},
        theta => {},
        {
          disableUnitFilterControls: true
        }
      ).then(instance => {
        input.oninput = evt => {
          const value = Number(input.value);
          instance.overrideFilter(unit => unit.squareFeet >= value);
        };
      });
    </script>
  </body>
```

### Customization

##### Disabling built in control menus

Use the `disableUnitFilterControls` flag on the ICustomOptions interface when invoking the library

##### Force reel into a specific mode

Use the `mode` flag on the ICustomOptions interface.
Available modes:
`all` -- **DEFAULT** no mode enforcement, users can use built-in controls to switch between amenities and unit views.
`units` -- forces the viewer into a unit search mode only, amenities are disabled
`amenities` --- forces the viewer into an amenity/transition mode only, unit search is disabled

##### Force reel to start at a specific frame

Use the `initialFrameNumber` field on the ICustomOptions interface.

### Help! How do I get a token?

Contact your account manager at Evolution Virtual for one. Once all necessary tour assets have been prepared (the building 3d, labels, transitions, etc) a token will be generated and provided to you for integration.
