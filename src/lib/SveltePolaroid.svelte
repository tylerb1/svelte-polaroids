<script>
	import SinglePolaroid from "./SinglePolaroid.svelte";
  let photos = [];
  let horizontal = false;
  let p = [];
  let j = 0;
  export let size = 250;
  export let sources = [
    'https://upload.wikimedia.org/wikipedia/commons/a/aa/Flat_Coated_Retriever_-_black.jpg',
    'https://upload.wikimedia.org/wikipedia/commons/6/69/Ansdell_Richard_The_Gamekeeper.jpg',
    'https://upload.wikimedia.org/wikipedia/commons/b/be/Blue_merle_Border_Collie.jpg',
    'https://upload.wikimedia.org/wikipedia/commons/thumb/9/9c/Ana%27s_Dog.jpg/1024px-Ana%27s_Dog.jpg',
    'https://upload.wikimedia.org/wikipedia/commons/thumb/a/af/2017-10-16_Labradoodle_Berlin.jpg/800px-2017-10-16_Labradoodle_Berlin.jpg',
  ];

  function getRandomTilt(horizontal, pos) {
    const normalTilt = Math.floor((8 + Math.random() * 8) * pos);
    const noTilt = Math.random() * 5 > 4;
    return horizontal
      ? Math.random() > 0.5
        ? 4
        : -4
      : noTilt 
        ? Math.random() > 0.5
          ? 2
          : -2
        : normalTilt
  }

  for (let i = 0; i < sources.length; i++) {
    let imgLoader = new Image();
    imgLoader.onload = function() {
      const height = imgLoader.height;
      const width = imgLoader.width;
      if (height < width) {
        horizontal = true;
      } else {
        horizontal = false;
      }
      const evenodd = j % 2 === 0 ? 1 : -1;
      const tilt = getRandomTilt(horizontal, evenodd);
      p.push({ 
        path: sources[i], 
        tilt,
        orientation: horizontal ? 'h' : 'v',
        z: evenodd + 2
      });
      j++;
      if (j === sources.length) {
        photos = p;
      }
    }
    imgLoader.src = sources[i];
  }
</script>        

<div class="parent">
  <div class="polaroid-container">
    {#each photos as photo, i}
      <SinglePolaroid 
        photo={photo} 
        size={size}
        isCurrent={i === 0} 
        z={photos.length - i}
      />
    {/each}
  </div>
</div> 

<style>
.polaroid-container {
  height: 300px;
  width: 200px;
  position: relative;
}

.parent {
  display: flex;
  justify-content: center;
  align-items: start;
}
</style>