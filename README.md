# svelte-polaroids

- a simple polaroid photo component 
- made with <3 by [tylerb1](https://github.com/tylerb1) 
- uses interactjs

# gif demo

![demo](https://i.imgur.com/DBpx36P.gif)

# usage

```
<script>
	import SveltePolaroid from "svelte-polaroids";
</script>

<SveltePolaroid size={200} sources={[ 'https://.../pic1.jpg', 'https://.../pic2.jpg', ... ]} />
```

# props

- size: maximum for the larger of the image's two dimensions
- sources: urls of the images to be used