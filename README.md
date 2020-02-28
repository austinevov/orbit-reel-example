### Documentation

To initialize the library, call:

`orbitCreelCreateFromUrsula(container, token, onUnitClicked)`

#####container
a reference to the HTMLDivElement the library will inject into. The parent container must have an aspect ratio of 16:9 (in other words, the height must be 9/16 the width of the container). The library will fill to 100% of the width/height of the container.

#####token
This will be provided.

#####onUnitClicked
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

###Getting Started

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
