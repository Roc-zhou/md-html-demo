<template>
  <div class="hello">
    <h1>{{ msg }}</h1>

    <div class="editor">
      <!-- <mavon-editor v-model="value" @change="changeMd" :ishljs="true" /> -->
    </div>
    <hr />
    <div class="content">
      <canvas id="demo-canvas"></canvas>
      <h1 class="main-title">
        Connect
        <span class="thin">Htmleaf</span>
      </h1>
    </div>
  </div>
</template>
script
<script>
import { mavonEditor } from "mavon-editor";
import "mavon-editor/dist/css/index.css";
const Circ = require("../assets/EasePack.min.js");
const { TweenLite } = require("../assets/TweenLite.min.js");
import aniFun from "../assets/rAF";
export default {
  name: "HelloWorld",
  components: {
    mavonEditor
  },
  data() {
    return {
      msg: "Welcome to Your Vue.js App",
      value: "",
      width: null,
      height: null,
      largeHeader: null,
      canvas: null,
      ctx: null,
      points: [],
      target: null,
      animateHeader: true
    };
  },
  mounted() {
    aniFun();
    this.init();
    this.initAnimation();
    this.addListeners();
  },
  methods: {
    changeMd(v, r) {
      console.log(r);
    },
    init() {
      this.largeHeader = document.querySelector(".content");
      this.width = this.largeHeader.clientWidth;
      this.height = this.largeHeader.clientHeight;

      this.target = { x: this.width / 2, y: this.height / 2 };

      this.canvas = document.getElementById("demo-canvas");
      this.canvas.width = this.width;
      this.canvas.height = this.height;
      this.ctx = this.canvas.getContext("2d");

      // 创建点
      this.points = [];
      for (let x = 0; x < this.width; x = x + this.width / 20) {
        for (let y = 0; y < this.height; y = y + this.height / 20) {
          let px = x + (Math.random() * this.width) / 20;
          let py = y + (Math.random() * this.height) / 20;
          let p = { x: px, originX: px, y: py, originY: py };
          this.points.push(p);
        }
      }

      // 对于每个点，找到5个最接近的点
      for (let i = 0; i < this.points.length; i++) {
        let closest = [];
        let p1 = this.points[i];
        for (let j = 0; j < this.points.length; j++) {
          let p2 = this.points[j];
          if (!(p1 == p2)) {
            let placed = false;
            for (let k = 0; k < 5; k++) {
              if (!placed) {
                if (closest[k] == undefined) {
                  closest[k] = p2;
                  placed = true;
                }
              }
            }

            for (let k = 0; k < 5; k++) {
              if (!placed) {
                if (
                  this.getDistance(p1, p2) < this.getDistance(p1, closest[k])
                ) {
                  closest[k] = p2;
                  placed = true;
                }
              }
            }
          }
        }
        p1.closest = closest;
      }
      //为每个点分配一个圆
      const Circle = this.Circle;
      for (let i in this.points) {
        let c = new Circle(
          this,
          this.points[i],
          2 + Math.random() * 2,
          "rgba(255,255,255,0.3)"
        );

        this.points[i].circle = c;
      }
    },
    getDistance(p1, p2) {
      return Math.pow(p1.x - p2.x, 2) + Math.pow(p1.y - p2.y, 2);
    },
    addListeners() {
      if (!("ontouchstart" in window)) {
        // window.addEventListener("mousemove", this.mouseMove);
      }
      // window.addEventListener("scroll", scrollCheck);
      window.addEventListener("resize", this.resize);
    },
    mouseMove(e) {
      let posx = 0,
        posy = 0;
      if (e.pageY && e.pageY < this.height) {
        posx = e.pageX;
        posy = e.pageY - 80;
      } else {
        posx = e.clientX;
        posy = this.height;
      }
      this.target.x = posx;
      this.target.y = posy;
    },
    resize() {
      this.canvas.width = this.largeHeader.clientWidth;
      this.canvas.height = this.largeHeader.clientHeight;
      this.initAnimation();
    },
    Circle(c_this, pos, rad, color) {
      let _this = this;
      // constructor
      (function() {
        _this.pos = pos || null;
        _this.radius = rad || null;
        _this.color = color || null;
      })();
      _this.draw = () => {
        if (!this.active) return;
        c_this.ctx.beginPath();
        c_this.ctx.arc(
          this.pos.x,
          this.pos.y,
          this.radius,
          0,
          2 * Math.PI,
          false
        );
        c_this.ctx.fillStyle = "rgba(156,217,249," + this.active + ")";
        c_this.ctx.fill();
      };
    },
    initAnimation() {
      this.animate();
      for (var i in this.points) {
        this.shiftPoint(this.points[i]);
      }
    },
    animate() {
      const { width, height, animateHeader, target } = this.$data;
      if (animateHeader) {
        this.ctx.clearRect(0, 0, width, height);
        for (let i in this.points) {
          // 检测范围内的点
          if (Math.abs(this.getDistance(target, this.points[i])) < 4000) {
            this.points[i].active = 0.3;
            this.points[i].circle.active = 0.6;
          } else if (
            Math.abs(this.getDistance(target, this.points[i])) < 20000
          ) {
            this.points[i].active = 0.1;
            this.points[i].circle.active = 0.3;
          } else if (
            Math.abs(this.getDistance(target, this.points[i])) < 40000
          ) {
            this.points[i].active = 0.02;
            this.points[i].circle.active = 0.1;
          } else {
            this.points[i].active = 0;
            this.points[i].circle.active = 0;
          }

          this.drawLines(this.points[i]);
          this.points[i].circle.draw();
        }
      }
      requestAnimationFrame(this.animate);
    },
    shiftPoint(p) {
      TweenLite.to(p, 1 + 1 * Math.random(), {
        x: p.originX - 50 + Math.random() * 100,
        y: p.originY - 50 + Math.random() * 100,
        ease: Circ.easeInOut,
        onComplete: () => {
          this.shiftPoint(p);
        }
      });
    },
    drawLines(p) {
      if (!p.active) return;
      for (let i in p.closest) {
        this.ctx.beginPath();
        this.ctx.moveTo(p.x, p.y);
        this.ctx.lineTo(p.closest[i].x, p.closest[i].y);
        this.ctx.strokeStyle = "rgba(156,217,249," + p.active + ")";
        this.ctx.stroke();
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.content {
  height: 500px;
  background: #666;
  position: relative;
}
.main-title {
  position: absolute;
  text-align: center;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  z-index: 2;
}
</style>
