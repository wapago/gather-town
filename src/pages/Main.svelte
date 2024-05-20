<script>
  import { onMount } from "svelte";
  import { getDatabase, ref, set, onValue } from "firebase/database";

  const db = getDatabase();
  const locationRef = ref(db, "location/");

  $: location = [];
  let canvas;
  let ctx;

  let sprite = new Image();
  sprite.src = "../src/sprite.png";

  let spriteX = 270;
  let spriteY = 270;
  let speed = 5;

  onMount(() => {
    writeUserData(spriteX, spriteY);
    ctx = canvas.getContext("2d");

    sprite.onload = function () {
      draw(spriteX, spriteY);
    };

    onValue(locationRef, (snapshot) => {
      const data = snapshot.val();
      location = Object.values(data);
      draw(location[0], location[1]);
    });
  });

  function writeUserData(spriteX, spriteY) {
    set(ref(db, "location/"), {
      spriteX: spriteX,
      spriteY: spriteY,
    });
  }

  document.addEventListener("keydown", function (event) {
    switch (event.keyCode) {
      case 37:
        spriteX -= speed;
        writeUserData(spriteX, spriteY);
        break;
      case 38:
        spriteY -= speed;
        writeUserData(spriteX, spriteY);
        break;
      case 39:
        spriteX += speed;
        writeUserData(spriteX, spriteY);
        break;
      case 40:
        spriteY += speed;
        writeUserData(spriteX, spriteY);
        break;
    }
  });

  function draw(spritX, spritY) {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    ctx.drawImage(sprite, spritX, spritY, 50, 50);
  }

  canvas = document.getElementById("myCanvas");
</script>

<body>
  <canvas bind:this={canvas} width="600" height="600"></canvas>
</body>

<style>
  canvas {
    border: 1px solid black;
  }
</style>
