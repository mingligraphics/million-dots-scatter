<script>
    import { getContext, onMount, onDestroy, afterUpdate } from 'svelte';
  
    export let font;
    export let textAlign;
    export let textBaseline;
    export let color;
    export let text;
    export let text_x;
    export let text_y;
    export let contextName = 'canvas';
  
    const { register, deregister, invalidate } = getContext(contextName);
  
  
    function draw(ctx) {
        ctx.font = font;
        ctx.textAlign = textAlign;
        ctx.textBaseline = textBaseline;
        ctx.fillStyle = color;  // a color name or by using rgb/rgba/hex values
        ctx.fillText(text, text_x, text_y); // text and position
    }
  
    onMount(() => {
      register(draw);
      invalidate();
    });
  
    onDestroy(() => {
      deregister(draw);
    });
  
    afterUpdate(invalidate);
  </script>