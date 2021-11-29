<template>
  <canvas id="triangleCanvas" width="500" height="500" style="border: 1px solid black"/>
</template>

<script>
import numeric from "numeric";

export default {
  name: "App",
  mounted() {
    var c = document.getElementById("triangleCanvas");
    var ctx = c.getContext("2d");
    c.style.transform = "scaleY(-1)";
    const width = 500,
      scale = 50;

    var A = [2, 4, 1],
      B = [4, 6, 1],
      C = [2, 6, 1],
      h = 0.5,
      k = 4;

    var T = [A, B, C],
      S = [
        [1, 0, 0],
        [0, 1, 0],
        [0, -h * k, 1],
      ],
      a = Math.atan(h),
      V = [
        [Math.cos(a), -Math.sin(a), 0],
        [Math.sin(a), Math.cos(a), 0],
        [0, 0, 1],
      ],
      R = [
        [1, 0, 0],
        [0, -1, 0],
        [0, 0, 1],
      ],
      res = numeric.dot(
        numeric.dot(
          numeric.dot(numeric.dot(numeric.dot(T, S), V), R),
          numeric.inv(V)
        ),
        numeric.inv(S)
      );

    ctx.clearRect(0, 0, 500, 500);

    // прямая
    const line = (x) => h * (x / scale + k) * scale;

    ctx.beginPath();
    ctx.moveTo(0, line(0));
    ctx.lineTo(width, line(width));
    ctx.stroke();
    ctx.closePath();

    // треугольник
    ctx.beginPath();
    ctx.moveTo(T[0][0] * scale, T[0][1] * scale);
    ctx.lineTo(T[1][0] * scale, T[1][1] * scale);
    ctx.lineTo(T[2][0] * scale, T[2][1] * scale);
    ctx.lineTo(T[0][0] * scale, T[0][1] * scale);
    ctx.stroke();
    ctx.closePath();

    console.log(res);
    // треугольник
    ctx.beginPath();
    ctx.moveTo(res[0][0] * scale, res[0][1] * scale);
    ctx.lineTo(res[1][0] * scale, res[1][1] * scale);
    ctx.lineTo(res[2][0] * scale, res[2][1] * scale);
    ctx.lineTo(res[0][0] * scale, res[0][1] * scale);
    ctx.stroke();
    ctx.closePath();
  },
};
</script>