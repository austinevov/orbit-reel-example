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

### Interface Documentation

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
	onUnitClicked?: (unit: IUnit) => void
}
```

### API Documentation

If your development already uses Ursula as a backend dependency, then the library can automatically pull most building information from Ursula without requiring manual input, thus simplifying the API entry point:

```
orbitReelCreateFromUrsula(
	container: HTMLDivElement,
	token: string,
	onUnitClicked?: (unit: IUnit) => void
)
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
  externalOptions: IOrbitReelOptions
)
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
		}
	}
);
```

### Help! How do I get a token?

Contact your account manager at Evolution Virtual for one. Once all necessary tour assets have been prepared (the building 3d, labels, transitions, etc) a token will be generated and provided to you for integration.
