<div 
  class="card" 
  data-dragging="false" 
  data-status="{isCurrent ? 'current' : 'waiting'}"
  style="z-index: {z};"
>
  <div 
    class="frame {photo.orientation}" 
    style="
      transform: rotate({photo.tilt}deg) 
        translateX({photo.orientation == 'h' ? '-30%' : 0})
        translateY({photo.orientation == 'h' ? '40%' : 0}); 
      z-index: {photo.z};
    "
    role="button"
  >
    <div 
      class="pic-fallback" 
      style={photo.orientation === 'h' ? `width: ${size}px; height: auto;` : `height: ${size}px; width: auto;`}
    >
      <img 
        class="polaroid"
        style={photo.orientation === 'h' ? `width: ${size}px; height: auto;` : `height: ${size}px; width: auto;`}
        alt="" 
        src={photo.path} 
        draggable="false"
      >
    </div>
  </div>
</div>

<script>
  import interact from 'interactjs'
  import { onMount } from "svelte";

	export let isCurrent;
	export let photo;
  export let z;
  export let size = 250;

  const interactThreshold = 100;
	const interactMaxRotation = 15;
	let rotation = 0;
	let x = 0;
	let y = 0;
	
  // https://hankchizljaw.com/wrote/get-css-custom-property-value-with-javascript/#heading-the-getcsscustomprop-function
	const getCSSCustomProp = (
    propKey, 
    element = document.documentElement, 
    castAs = 'string'
  ) => {
		let response = getComputedStyle(element).getPropertyValue(propKey);
		if (response.length) {
			response = response.replace(/\'|"/g, '').trim();
		}
		switch (castAs) {
			case 'number':
			case 'int':
				return parseInt(response, 10);
			case 'float':
				return parseFloat(response, 10);
			case 'boolean':
			case 'bool':
				return response === 'true' || response === '1';
		}
		return response;
	};

	function dragMoveListener(event) {
		var target = event.target
		x = (getCSSCustomProp('--card-x', target, 'float') || 0) + (event.dx / 5)
		y = (getCSSCustomProp('--card-y', target, 'float') || 0) + (event.dy / 5)
		rotation = interactMaxRotation * (x / interactThreshold);
		if (rotation > interactMaxRotation) {
      rotation = interactMaxRotation;
    } else if (rotation < -interactMaxRotation) {
      rotation = -interactMaxRotation;
    }
		target.style.setProperty('--card-x', x + 'px');
		target.style.setProperty('--card-y', y + 'px');
		target.style.setProperty('--card-r', rotation + 'deg');
	}

  onMount(() => {
    const vw = document.documentElement.clientWidth;
    let offX = 400;
    if (vw > 400) {
      offX = vw;
    }
    interact('.card[data-status="current"]').draggable({
      onstart: (event) => {
        event.target.setAttribute('data-dragging', true);
      },
      onmove: dragMoveListener,
      onend: (event) => {
        event.target.setAttribute('data-dragging', false);
        let moved = (Math.sqrt(Math.pow(event.pageX - event.x0, 2) + 
          Math.pow(event.pageY - event.y0, 2) | 0));
        if (moved > interactThreshold) {
          event.target.setAttribute('data-status', "transition");
          if (x > 0) {
            x = offX * 2;
          } else {
            x = (offX * -2);
          }
          event.target.addEventListener('transitionend', () => {
            event.target.setAttribute('data-status', "done");
          });
          event.target.nextElementSibling?.setAttribute('data-status', 'current');
        }
        else {
          x = 0;
          y = 0;
          rotation = 0;
          event.target.style.setProperty('--card-r', rotation + 'deg');
        }
        event.target.style.setProperty('--card-x', x + 'px');
        event.target.style.setProperty('--card-y', y + 'px');
      }
    });
  });
</script>

<style>
:root {
  --polaroid-frame-shadow: 0px 1px 2px 0px rgba(0, 0, 0, 0.25) inset, 0px 2px 6px 0px rgba(0, 0, 0, 0.50);
  --polaroid-image-shadow: 0px 1px 1px 0px rgba(0, 0, 0, 0.3) inset;
}

.frame {
  aspect-ratio: auto;
  border-radius: 3px;
  background: var(--background-white);
  box-shadow: var(--polaroid-frame-shadow);
  background-color: #fff; 
  display: inline-block;
  transition: transform 0.2s ease;
  touch-action: none;
}

.card[data-dragging="false"] {
	transition: transform 0.5s;
}

.card[data-status="current"] {
	z-index: 500;
}

.card {
  --card-x: 0;
  --card-y: 0;
  --card-r: 0;
  transform: translate(var(--card-x),var(--card-y)) rotate(var(--card-r)) scale(0.7);
  top: -10%;
  left: 0%;
  position: absolute;
}

.v {
  padding: 15px 15px 70px 15px;
}

.h {
  padding: 15px 70px 15px 15px;
}

.polaroid {
  aspect-ratio: auto;
  border-radius: 1px;
  box-shadow: var(--polaroid-image-shadow);
}

.pic-fallback { 
  background-color: #eee;
}
</style>