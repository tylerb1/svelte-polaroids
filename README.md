# svelte-polaroids

- a simple polaroid photo component
- great for letting users browse sequential images
- only dependency is interactjs

# gif demo

![demo](https://i.imgur.com/lfpSrrL.gif)

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

# authors

- written by [tylerb1](https://github.com/tylerb1) 
- designed by [Jennifer Herzberg](https://www.jenniferherzberg.com)
- adopted from a [demo](https://smth.uk/building-a-swipeable-card-stack/) by Sam Smith
