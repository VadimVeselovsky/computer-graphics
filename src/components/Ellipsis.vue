<template>
  <div>
    <canvas id="ellipsisCanvas" width="500" height="500" style="scaleY(-1); border: 1px solid black"/>
    <div style="text-align: center; display: grid; grid-row-gap: 8px">
      <div>
        <label for="params_a">a</label>
        <input type="number" v-model="params.a" id="params_a" />
      </div>
      <div>
        <label for="params_b">b</label>
        <input type="number" v-model="params.b" id="params_b" />
      </div>
      <div>
        <label for="params_n">Число узлов</label>
        <input type="number" v-model="params.n" id="params_n" min="2"/>
      </div>
      <div>
        <label for="params_angle">Угол, в градусах</label>
        <input type="number" v-model="params.angle" id="params_n" />
      </div>
      <div>
        <div style="margin-bottom: 4px;">Смещение</div>
        <label for="params_hx">x</label>
        <input type="number" v-model="params.hx" id="params_hx" />
        <label style="margin-left: 8px" for="params_hy">y</label>
        <input type="number" v-model="params.hy" id="params_hy" />
      </div>
    </div>
  </div>
</template>

<script>
import numeric from "numeric";

/** 1. Получить матрицу с точками эллипса
 *  2. Преобразовать всякими трансформациями
 */

function toCanvasCoords(x, y, width = 500, height = 500) {
  const scale = 50
  return [width / 2 + scale * x, height / 2 + scale * y];
}

export default {
  name: "App",

  data() {
    return {
      params: {
        a: 1,
        b: 2,
        n: 16,
        angle: 0,
        hx: 0,
        hy: 0
      },
    };
  },

  methods: {
    update() {
      var c = document.getElementById("ellipsisCanvas");
      var ctx = c.getContext("2d");
      c.style.transform = "scaleY(-1)";

      var a = this.params.a,
        b = this.params.b,
        n = this.params.n,
        x = (phi) => a * Math.cos(phi),
        y = (phi) => b * Math.sin(phi),
        phiFrom = 0,
        phiTo = 2 * Math.PI,
        T = [];

      for (var i = 0; i < n; i++) {
        var phi = (i / n) * phiTo + (1 - i / n) * phiFrom;
        T.push([x(phi), y(phi), 1]);
      }

      var a = this.params.angle/360*Math.PI*2,
        hx = this.params.hx,
        hy = this.params.hy,
        S = [
          [1, 0, 0],
          [0, 1, 0],
          [hx, hy, 1],
        ],
        V = [
          [Math.cos(a), -Math.sin(a), 0],
          [Math.sin(a), Math.cos(a), 0],
          [0, 0, 1],
        ],
        res = numeric.dot(numeric.dot(T, V), S);

      ctx.clearRect(0, 0, 500, 500);

      // треугольник
      ctx.beginPath();
      ctx.moveTo(...toCanvasCoords(res[0][0], res[0][1]));
      for (var i = 1; i <= n; i++) {
        ctx.lineTo(...toCanvasCoords(res[i % n][0], res[i % n][1]));
      }
      ctx.stroke();
      ctx.closePath();
    },
  },

  mounted() {
    this.update();
  },

  watch: {
    params: {
      deep: true,
      handler: "update",
    },
  },
};
</script>