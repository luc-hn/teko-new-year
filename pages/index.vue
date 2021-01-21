<template>
  <!-- <div class="container">
   <div>
     <img class="image" :src="logo" />
     <div class="text">Something new is going to deploy</div>
   </div>
  </div> -->
  <div>
    <canvas id="canvas">Canvas not supported.</canvas>
    <div class="controller">
      <input type="range" id="count" min="0" max="50" step="1" value="10">
    </div>
  </div>
</template>

<script>
(function() {
  'use strict';
  window.addEventListener('load', function() {
    var count = document.getElementById('count');
    var countBall = count.value;
    if( /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent) ) {
      countBall = Math.round(count.value / 3);
    }
    var canvas = document.getElementById('canvas');

    if (!canvas || !canvas.getContext) {
      return false;
    }

    /********************
      Random Number
    ********************/

    function rand(min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min);
    }

    /********************
      Var
    ********************/

    // canvas 
    var ctx = canvas.getContext('2d');
    var X = canvas.width = window.innerWidth;
    var Y = canvas.height = window.innerHeight;

    /********************
      Animation
    ********************/

    window.requestAnimationFrame =
      window.requestAnimationFrame ||
      window.mozRequestAnimationFrame ||
      window.webkitRequestAnimationFrame ||
      window.msRequestAnimationFrame ||
      function(cb) {
        setTimeout(cb, 17);
      };

    /********************
      Ball
    ********************/
    
    // var
    var ballNum = countBall;
    var balls = [];
     
    function Ball(ctx, x, y) {
      this.ctx = ctx;
      this.init(x, y);
    }

    Ball.prototype.init = function(x, y) {
      this.x = x;
      this.y = y;
      this.r = rand(1, 3);
      this.v = {
        y: rand(1, 5) * 1.2
      };
      this.bloomY = rand(0, Y / 2);
    };

    Ball.prototype.draw = function() {
      var ctx = this.ctx;
      ctx.beginPath();
      ctx.fillStyle = 'rgb(153, 153, 153)';
      ctx.arc(this.x, this.y, this.r, 0, Math.PI * 2, false);
      ctx.fill();
    };

    Ball.prototype.wrapPosition = function() {
      if (this.y < this.bloomY) {
        bloomFire(ctx, this.x, this.y, getColor(), rand(0.8, 3));
        this.y = Y;
        this.x = rand(0, X);
      }
    };

    Ball.prototype.updatePosition = function() {
      this.y -= this.v.y;
    };

    Ball.prototype.resize = function() {
      this.x = rand(0, X);
    };

    Ball.prototype.render = function() {
      this.updatePosition();
      this.wrapPosition();
      this.draw();
    };

    for (var i = 0; i < ballNum; i++) {
      var ball = new Ball(ctx, rand(0, X), Y);
      balls.push(ball);
    }

    /********************
      Fire
    ********************/
    
    // var
    var fires = [];
    var fireGravity = 0.01;

    function getColor() {
      // var color = "rgb(" + rand(0, 255) + ',' + rand(0, 255) + ',' + rand(0, 255) + ")";
      return "#925F2A";
    }
    function Fire(ctx, x, y, c, d, r) {
      this.ctx = ctx;
      this.init(x, y, c, d, r);
    }

    Fire.prototype.init = function(x, y, c, d, r) {
      this.rad = r * Math.PI / 180;
      this.x = x;
      this.y = y;
      this.r = 2;
      this.c = c;
      this.d = d;
      this.l = rand(10, 20);
      this.v = {
        x: Math.cos(this.rad) * this.d,
        y: Math.sin(this.rad) * this.d
      };
    };

    Fire.prototype.draw = function() {
      var ctx = this.ctx;
      ctx.beginPath();
      ctx.globalCompositeOperation = 'lighter';
      ctx.fillStyle = this.c;
      ctx.arc(this.x, this.y, this.r, 0, 2 * Math.PI);
      ctx.fill();
    };

    Fire.prototype.updatePosition = function() { 
      this.v.y += fireGravity;
      this.x += this.v.x;
      this.y += this.v.y;
    };

    Fire.prototype.updateParams = function() {
      this.l -= 0.1;
    };

    Fire.prototype.deleteFire = function(i) {
       if (this.l < 0) {
         fires.splice(i, 1);
       }
    };

    Fire.prototype.resize = function() {
      this.x = rand(0, X);
    };

    Fire.prototype.render = function(i) {
      this.updateParams();
      this.deleteFire(i);
      this.updatePosition();
      this.draw();
    };

    function bloomFire(ctx, x, y, c, d) {
      for (var i = 0; i < 36; i++) {
        var fire = new Fire(ctx, x, y, c, d, i * 10);
        fires.push(fire);
      }
    }
         
    /********************
      Render
    ********************/
    
    function render(){
      ctx.globalCompositeOperation = "darken";
      ctx.globalAlpha = 0.05;
      ctx.fillStyle = "rgb(0,0,0)";
      ctx.fillRect(0, 0, X, Y);
      ctx.globalCompositeOperation = "source-over";
      ctx.globalAlpha = 1;
      for (var i = 0; i < balls.length; i++) {
        balls[i].render();
      }
      for (var i = 0; i < fires.length; i++) {
        fires[i].render(i);
      }

      requestAnimationFrame(render);
    }

    render();

    /********************
      Event
    ********************/
    
    function onResize() {
      X = canvas.width = window.innerWidth;
      Y = canvas.height = window.innerHeight;
      for (var i = 0; i < fires.length; i++) {
        fires[i].resize();
      }
    }

    count.addEventListener('change', function() {
      balls = [];
      var val = this.value;
      ballNum = val;
      for (var i = 0; i < ballNum; i++) {
        var ball = new Ball(ctx, rand(0, X), Y);
        balls.push(ball);
      }
    });

    window.addEventListener('resize', function() {
      onResize();
    });

  }); 
})();
import logo from '~/assets/newyear.jpg'
import VuesaxLogo from '~/components/VuesaxLogo.vue'

export default {
  components: {
    VuesaxLogo,
  },
  data: function () {
      return {
          logo: logo
      }
  }
}
</script>

<style>
/* .text {
  font-size: 20px;
  font-style: italic;
}
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.image {
  width: 100%;
  height: auto;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 55px;
  color: #35495e;
  letter-spacing: 1px;
  text-transform: capitalize;
  margin: 25px 0;
}

.subtitle {
  font-weight: 300;
  font-size: 1.1rem;
  color: #526488;
  word-spacing: 2px;
  padding-bottom: 15px;
  max-width: 600px;
}

.subtitle a {
  font-weight: 500;
  color: inherit;
}

.links {
  padding-top: 15px;
  margin-bottom: 20px;
}

.content-logos {
  display: flex;
  align-items: center;
  justify-content: center;
  min-width: 500px;
}

.plus {
  font-size: 2.5rem;
  margin: 15px;
  color: #35495e;
}

.h3 {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  font-weight: 400;
  margin: 10px;
} */
* {
  margin: 0;
  padding: 0;
}

canvas#canvas {
  display: block;
  background: #000;
}

div.controller {
  position: absolute;
  bottom: 0;
  left: 0;
  padding: 1.6rem;
  display: none;
}
</style>
