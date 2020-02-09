<template>
  <div class="circledemo">
    <!--   <canvas
      :width="canvas.width"
      :height="canvas.height"
      ref="cicledemo"
      @mouseover="stopfun"
      @mouseout="startfun"
    ></canvas>-->
    <canvas :width="canvas.width" :height="canvas.height" ref="cicledemo"></canvas>
  </div>
</template>

<script>
export default {
  name: "circledemo",
  data() {
    return {
      canvas: {
        width: 800,
        height: 800
      },
      timerval: null,
      i: 0
    };
  },
  mounted() {
    this.timer();
  },
  methods: {
    //鼠标移入动画暂停
    stopfun() {
      cancelAnimationFrame(this.timerval);
    },
    //鼠标移出动画开始
    startfun() {
      this.timerval = window.requestAnimationFrame(this.timer);
    },
    //动画函数
    timer() {
      this.i++;
      var width = document.body.clientWidth;
      var height = document.body.clientHeight;
      this.canvas = { width, height };
      let canvas = this.$refs.cicledemo;
      let ctx = canvas.getContext("2d");
      ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);
      console.log(this.canvas);
      //绘制所有动画canvas
      this.draw(ctx);
      console.log(this.i);
      //帧动画开始
      cancelAnimationFrame(this.timerval); //清除帧动画
      this.timerval = window.requestAnimationFrame(this.timer);
    },
    draw(ctx) {
      this.drawreactbg(ctx); //绘制背景矩形
      this.drawcirclebg(ctx); //绘制背景圆形
      this.drawcirclenum(ctx); //绘制外层表盘数字
      this.drawrealdatacircle(ctx); //绘制最外层实际数据圆环
      this.drawrealdatacirclecenter(ctx); //绘制中间层实际数据圆环
      this.drawcirclebg2(ctx); //绘制中间层数字外层背景圆形
      this.drawcirclenum2(ctx); //绘制内层表盘数字
      this.drawcirclebg3(ctx); //绘制中间层数字内层背景圆形
      this.drawrealdatacirclesmall(ctx); //绘制内层实际数据圆环
      this.drawrealtotaldata(ctx); //绘制（实际汇总数据)
      this.drawcirclebg4(ctx); //绘制最里层层数字内层背景圆环
      //this.drawtext(ctx)  //绘制(加文字)
      this.drawrotatecirclebig(ctx); //绘制旋转虚线圆环（最外层）
      this.drawrotatecirclecenter(ctx); //绘制旋转虚线圆环（中间层）
      this.drawrotatecirclesmall(ctx); //绘制旋转虚线圆环（最里层）
      this.saomaosanxing(ctx); //旋转扫描扇形
      this.corvercircle(ctx); //遮挡扫描扇形实心圆
      this.jianbiancircle(ctx); //旋转渐变圆弧
    },
    drawreactbg(ctx) {
      //绘制背景矩形
      ctx.save();
      //ctx.translate(this.canvas.width /2, this.canvas.height / 2);
      ctx.fillStyle = "#1b1b1b";
      ctx.fillRect(0, 0, this.canvas.width, this.canvas.height);
      ctx.restore();
    },
    drawcirclebg(ctx) {
      //绘制背景圆形
      ctx.save();
      ctx.translate(this.canvas.width / 2, this.canvas.height / 2);
      ctx.beginPath();
      ctx.arc(0, 0, this.canvas.height / 3, 0, Math.PI * 2);
      ctx.closePath();
      // 用渐变进行填充颜色
      ctx.strokeStyle = "#343F4A";
      // 设置圆环的宽度
      ctx.lineWidth = 6;
      ctx.fillStyle = "#292934"; //填充圆
      ctx.fill();
      ctx.stroke();
      ctx.restore();
    },
    //绘制外层表盘数字
    drawcirclenum(ctx) {
      ctx.save();
      ctx.translate(this.canvas.width / 2, this.canvas.height / 2);
      ctx.beginPath();
      var hourNumbers = ["15", "18", "21", "24"]; //定义标记小时的数组
      hourNumbers.map((number, i) => {
        //遍历 取出各刻度及所在索引
        //每个刻度所占弧度为索引乘以一小时的弧度，即 1点钟30度，2点钟60度，以此类推..
        var rad = ((2 * Math.PI) / 4) * i;
        var x = Math.cos(rad) * this.canvas.height * 0.32;
        var y = Math.sin(rad) * this.canvas.height * 0.32; //确定各刻度点 X、Y坐标
        ctx.textAlign = "center"; //绘制的刻度在整个画布左右居中
        ctx.textBaseline = "middle"; //同理，上下居中
        ctx.font = "12px Arial";
        ctx.fillStyle = "#fff"; //设置显示刻度的数字 1,2,3.. 的字体及字号
        ctx.fillText(number, x, y); //绘制文字
      });

      ctx.closePath();
      ctx.restore();
    },
    //绘制最外层实际数据圆环
    drawrealdatacircle(ctx) {
      ctx.save();
      ctx.translate(this.canvas.width / 2, this.canvas.height / 2);
      for (let i = 0; i < 360; i++) {
        /*  */
        if (i % 30 == 0) {
          var grd = `red`;
        } else if (i % 40 == 0) {
          var grd = `blue`;
        } else {
          var grd = "#245274";
        }
        /*  */
        let angel = -90 + i * (360 / 360); //角度
        let [x1, y1] = [
          Math.cos((angel * Math.PI) / 180) * this.canvas.height * 0.3,

          Math.sin((angel * Math.PI) / 180) * this.canvas.height * 0.3
        ];
        let [x2, y2] = [
          Math.cos((angel * Math.PI) / 180) * this.canvas.height * 0.28,

          Math.sin((angel * Math.PI) / 180) * this.canvas.height * 0.28
        ];
        ctx.beginPath();
        ctx.moveTo(x1, y1);
        ctx.lineTo(x2, y2);
        ctx.closePath();
        //
        //ctx.strokeStyle = "#245274";
        ctx.strokeStyle = grd;
        ctx.lineWidth = 2;
        ctx.lineCap = "round";
        ctx.stroke();
      }
      ctx.restore();
    },
    //绘制中间层实际数据圆环
    drawrealdatacirclecenter(ctx) {
      ctx.save();
      ctx.translate(this.canvas.width / 2, this.canvas.height / 2);
      for (let i = 0; i < 360; i++) {
        let angel = -90 + i * (360 / 360); //角度
        let [x1, y1] = [
          Math.cos((angel * Math.PI) / 180) * this.canvas.height * 0.27,

          Math.sin((angel * Math.PI) / 180) * this.canvas.height * 0.27
        ];
        let [x2, y2] = [
          Math.cos((angel * Math.PI) / 180) * this.canvas.height * 0.23,

          Math.sin((angel * Math.PI) / 180) * this.canvas.height * 0.23
        ];
        ctx.beginPath();
        ctx.moveTo(x1, y1);
        ctx.lineTo(x2, y2);
        ctx.closePath();
        //
        ctx.strokeStyle = "#245274";
        ctx.lineWidth = 2;
        ctx.lineCap = "round";
        ctx.stroke();
      }
      ctx.restore();
    },
    drawcirclebg2(ctx) {
      //绘制中间层数字外层背景圆形
      ctx.save();
      ctx.translate(this.canvas.width / 2, this.canvas.height / 2);
      ctx.beginPath();
      ctx.arc(0, 0, this.canvas.height / 4.6, 0, Math.PI * 2);
      ctx.closePath();
      // 用渐变进行填充颜色
      ctx.strokeStyle = "#343F4A";
      // 设置圆环的宽度
      ctx.lineWidth = 6;
      // ctx.fillStyle = "#292934"; //填充圆
      // ctx.fill();
      ctx.stroke();
      ctx.restore();
    },
    //绘制内层表盘数字
    drawcirclenum2(ctx) {
      ctx.save();
      ctx.translate(this.canvas.width / 2, this.canvas.height / 2);
      ctx.beginPath();
      var hourNumbers = ["3", "6", "9", "12"]; //定义标记小时的数组
      hourNumbers.map((number, i) => {
        //遍历 取出各刻度及所在索引
        //每个刻度所占弧度为索引乘以一小时的弧度，即 1点钟30度，2点钟60度，以此类推..
        var rad = ((2 * Math.PI) / 4) * i;
        var x = Math.cos(rad) * this.canvas.height * 0.2;
        var y = Math.sin(rad) * this.canvas.height * 0.2; //确定各刻度点 X、Y坐标
        ctx.textAlign = "center"; //绘制的刻度在整个画布左右居中
        ctx.textBaseline = "middle"; //同理，上下居中
        ctx.font = "12px Arial";
        ctx.fillStyle = "#fff"; //设置显示刻度的数字 1,2,3.. 的字体及字号
        ctx.fillText(number, x, y); //绘制文字
      });

      ctx.closePath();
      ctx.restore();
    },
    drawcirclebg3(ctx) {
      //绘制中间层数字内层背景圆形
      ctx.save();
      ctx.translate(this.canvas.width / 2, this.canvas.height / 2);
      ctx.beginPath();
      ctx.arc(0, 0, this.canvas.height / 5.5, 0, Math.PI * 2);
      ctx.closePath();
      // 用渐变进行填充颜色
      ctx.strokeStyle = "#343F4A";
      // 设置圆环的宽度
      ctx.lineWidth = 6;
      // ctx.fillStyle = "#292934"; //填充圆
      // ctx.fill();
      ctx.stroke();
      ctx.restore();
    },
    //绘制内层实际数据圆环
    drawrealdatacirclesmall(ctx) {
      ctx.save();
      ctx.translate(this.canvas.width / 2, this.canvas.height / 2);
      for (let i = 0; i < 180; i++) {
        let angel = -90 + i * (360 / 180); //角度
        let [x1, y1] = [
          Math.cos((angel * Math.PI) / 180) * this.canvas.height * 0.17,

          Math.sin((angel * Math.PI) / 180) * this.canvas.height * 0.17
        ];
        let [x2, y2] = [
          Math.cos((angel * Math.PI) / 180) * this.canvas.height * 0.14,

          Math.sin((angel * Math.PI) / 180) * this.canvas.height * 0.14
        ];
        ctx.beginPath();
        ctx.moveTo(x1, y1);
        ctx.lineTo(x2, y2);
        ctx.closePath();
        //
        ctx.strokeStyle = "#245274";
        ctx.lineWidth = 1;
        ctx.lineCap = "round";
        ctx.stroke();
      }
      ctx.restore();
    },
    //绘制（实际汇总数据)
    drawrealtotaldata(ctx) {
      ctx.save();
      ctx.translate(this.canvas.width / 2, this.canvas.height / 2);
      for (let i = 0; i < 720; i++) {
        let angel = -90 + i * (360 / 720); //角度
        let [x1, y1] = [
          Math.cos((angel * Math.PI) / 180) * this.canvas.height * 0.13,

          Math.sin((angel * Math.PI) / 180) * this.canvas.height * 0.13
        ];
        let [x2, y2] = [
          Math.cos((angel * Math.PI) / 180) * this.canvas.height * 0.1,

          Math.sin((angel * Math.PI) / 180) * this.canvas.height * 0.1
        ];
        //
        ctx.strokeStyle = "#292934";
        if (i > 0 && i < 99) {
          ctx.strokeStyle = "#836B27";
        } else if (i > 103 && i < 359) {
          ctx.strokeStyle = "#4CC2CE";
        } else if (i > 369 && i < 712) {
          ctx.strokeStyle = "rgb(108,40,46)";
        } else {
          ctx.strokeStyle = "#292934";
        }
        //

        ctx.lineWidth = 3;
        ctx.lineCap = "round";
        ctx.beginPath();
        ctx.moveTo(x1, y1);
        ctx.lineTo(x2, y2);
        ctx.closePath();
        ctx.stroke();
      }
      ctx.restore();
    },
    drawcirclebg4(ctx) {
      //绘制最里层层数字内层背景圆环
      ctx.save();
      ctx.translate(this.canvas.width / 2, this.canvas.height / 2);
      ctx.beginPath();
      ctx.arc(0, 0, this.canvas.height / 11.2, 0, Math.PI * 2);
      ctx.closePath();
      /*  */
        var grd = `rgba(${parseInt(Math.random() * 255)},${parseInt(
        Math.random() * 255
      )},${parseInt(Math.random() * 255)})`;
      /*  */
      // 用渐变进行填充颜色
      ctx.strokeStyle = "#343F4A";
      //ctx.strokeStyle = grd
      // 设置圆环的宽度
      ctx.lineWidth = 6;
      // ctx.fillStyle = "#292934"; //填充圆
      // ctx.fill();
      ctx.stroke();
      ctx.restore();
    },
    /*  */
    drawrotatecirclebig(ctx) {
      //绘制旋转虚线圆环（最外层）
      ctx.save();
      ctx.translate(this.canvas.width / 2, this.canvas.height / 2);
      ctx.rotate((Math.PI / 500) * this.i + (Math.PI / 120) * this.i);
      ctx.beginPath();
      ctx.arc(0, 0, this.canvas.height / 2.2, 0, Math.PI * 2);
      ctx.closePath();
      ctx.strokeStyle = "red";
      ctx.lineWidth = 5;
      ctx.setLineDash([this.canvas.height / 2.2, this.canvas.height / 4.2]); // [实线长度, 间隙长度]
      ctx.lineDashOffset = this.canvas.height / 4.4;
      ctx.stroke();
      ctx.restore();
    },
    drawrotatecirclecenter(ctx) {
      //绘制旋转虚线圆环（中间层）
      ctx.save();
      ctx.translate(this.canvas.width / 2, this.canvas.height / 2);
      ctx.rotate(-(Math.PI / 500) * this.i + -(Math.PI / 120) * this.i);
      ctx.beginPath();
      ctx.arc(0, 0, this.canvas.height / 2.45, 0, Math.PI * 2);
      ctx.closePath();
      ctx.strokeStyle = "red";
      ctx.lineWidth = 5;
      ctx.setLineDash([this.canvas.height / 2.45, this.canvas.height / 4.9]); // [实线长度, 间隙长度]
      ctx.lineDashOffset = this.canvas.height / 4.9;
      ctx.stroke();
      ctx.restore();
    },
    drawrotatecirclesmall(ctx) {
      //绘制旋转虚线圆环（最里层）
      ctx.save();
      ctx.translate(this.canvas.width / 2, this.canvas.height / 2);
      ctx.rotate((Math.PI / 500) * this.i + (Math.PI / 120) * this.i);
      ctx.beginPath();
      ctx.arc(0, 0, this.canvas.height / 2.75, 0, Math.PI * 2);
      ctx.closePath();
      ctx.strokeStyle = "red";
      ctx.lineWidth = 5;
      ctx.setLineDash([this.canvas.height / 2.8, this.canvas.height / 5.4]); // [实线长度, 间隙长度]
      ctx.lineDashOffset = this.canvas.height / 5.4;
      ctx.stroke();
      ctx.restore();
    },
    saomaosanxing(ctx) {
      //旋转扫描扇形
      ctx.save();
      ctx.translate(this.canvas.width / 2, this.canvas.height / 2);
      ctx.rotate((Math.PI / 3000) * this.i + (Math.PI / 600) * this.i);
      //ctx.arc(0, 0, 160, 0 + this.i * 300, Math.PI/2 + this.i * 2);
      ctx.beginPath();
      ctx.moveTo(0, 0);
      ctx.arc(0, 0, this.canvas.height / 3, 0, Math.PI / 4);
      ctx.closePath();
      ctx.fillStyle = "rgba(255,255,255,0.02)";
      //ctx.fillStyle = "rgba(255,255,255,0.7)";
      ctx.fill();

      ctx.restore();
    },
    corvercircle(ctx) {
      //遮挡扫描扇形实心圆
      ctx.save();
      ctx.translate(this.canvas.width / 2, this.canvas.height / 2);
      ctx.beginPath();
      ctx.arc(0, 0, this.canvas.height / 12, 0, Math.PI * 2);
      ctx.closePath();
      ctx.fillStyle = "#292934";
      ctx.fill();
      ctx.restore();
      //this.corvercirclewarn(ctx)//遮挡扫描扇形实心圆(报警时)
      this.corvercirclewarnvideo(ctx)//遮挡扫描扇形实心圆(报警时音频声音)
      this.drawtext(ctx); //绘制(加文字)
    },
    corvercirclewarn(ctx) {
      //遮挡扫描扇形实心圆(报警时)
      ctx.save();
      ctx.translate(this.canvas.width / 2, this.canvas.height / 2);
      ctx.beginPath();
      ctx.arc(0, 0, this.canvas.height / 12, 0, Math.PI * 2);
      ctx.closePath();
      var grd = `rgba(${parseInt(Math.random() * 255)},${parseInt(
        Math.random() * 255
      )},${parseInt(Math.random() * 255)})`;
      ctx.fillStyle = grd;
      ctx.fill();
      ctx.restore();
    },
     corvercirclewarnvideo(ctx) {
      //遮挡扫描扇形实心圆(报警时音频声音)
      ctx.save();
      ctx.translate(this.canvas.width / 2, this.canvas.height / 2);
      ctx.beginPath();
      ctx.arc(0, 0, this.canvas.height / 12, 0, Math.PI * 2);
      ctx.closePath();

      ctx.fillStyle = "rgba(255,255,255,0)";
      ctx.fill();
      ctx.restore();
    },
    //绘制(加文字)
    drawtext(ctx) {
      ctx.save();
      var grd = `rgba(${parseInt(Math.random() * 255)},${parseInt(
        Math.random() * 255
      )},${parseInt(Math.random() * 255)})`;
      ctx.translate(this.canvas.width / 2, this.canvas.height / 2);
      ctx.beginPath();
      ctx.textAlign = "center";
      ctx.textBaseline = "middle";
      ctx.font = `${24}px MicrosoftYaHei`;
      //ctx.fillStyle = "#B4B4B4"; //填充文字颜色
      ctx.fillStyle = grd;
      ctx.fill();
      ctx.fillText("24", 0, -20);
      ctx.closePath();
      ctx.beginPath();
      ctx.textAlign = "center";
      ctx.textBaseline = "middle";
      ctx.font = `${20}px MicrosoftYaHei`;
      // ctx.fillStyle = "#6C6C6C"; //填充文字颜色
      ctx.fillStyle = grd;
      ctx.fill();
      ctx.fillText("报警总数", 0, 20);
      ctx.closePath();
      ctx.restore();
    },
    jianbiancircle(ctx) {
      //旋转渐变圆弧
      ctx.save();
      ctx.beginPath();
      ctx.translate(this.canvas.width / 2, this.canvas.height / 2);
      ctx.rotate((Math.PI / 3000) * this.i + (Math.PI / 600) * this.i);

      // ctx.arc(0, 0, 60, 0 + this.i * 2, Math.PI/2 + this.i * 2);
      ctx.arc(0, 0, this.canvas.height / 15.2, 0, Math.PI / 4);

      /*  var grd = ctx.createLinearGradient(0, 0, 170, 0); */

      var grd = ctx.createLinearGradient(0, 0, 170, 0);
      grd.addColorStop(0, "black");
      grd.addColorStop("0.3", "magenta");
      grd.addColorStop("0.5", "blue");
      grd.addColorStop("0.6", "green");
      grd.addColorStop("0.8", "yellow");
      grd.addColorStop(1, "red");

      //ctx.fillStyle = grd;
      // 用渐变进行填充
      ctx.strokeStyle = grd;
      ctx.lineWidth = 7;
      //ctx.lineCap = "round";
      ctx.stroke();
      ctx.closePath();
      ctx.restore();
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
@import "@/styles/global.scss";
.circledemo {
  display: flex;
  flex-flow: row nowrap;
  justify-content: center;
  align-items: center;
  width: vw(1920);
  height: vh(1080);
}
a {
  color: #42b983;
  color: #2c3e50;
}
</style>
