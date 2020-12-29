<template>
  <div>
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css"
    />
    <h1>Paint</h1>
    <div class="flex-container">
      <div class="border">
        <button class="btnshape" @click="dataToString">
          <i class="fa fa-save"></i>
        </button>
        <label class="btnshape" for="load">load</label>
        <input
          type="file"
          id="load"
          @change="load"
          class="input"
          accept="application/json,application/xml"
        />
        <button class="btnshape" @click="undo()">
          <i class="fa fa-undo"></i>
        </button>
        <button class="btnshape" @click="redo()">Redo</button><br />
        <button class="btnshape" @click="alerts('copy')">Copy</button>
        <button class="btnshape" @click="alerts('move')">Move</button>
        <button class="btnshape" @click="alerts('delete')">
          <i class="fa fa-trash"></i>
        </button>
        <button class="btnshape" @click="alerts('resize')">resize</button><br />
        Tools
      </div>
      <div class="border">
        <button class="btnshape" @click="alerts('drawLine')">line</button>
        <button class="btnshape" @click="alerts('drawCircle')">
          <i class="fa fa-circle-o"></i>
        </button>
        <button class="btnshape" @click="alerts('drawEllipse')">Ellipse</button
        ><br />
        <button class="btnshape" @click="alerts('drawTriangle')">
          Triangle
        </button>
        <button class="btnshape" @click="alerts('drawRectangle')">
          Rectangle
        </button>
        <button class="btnshape" @click="alerts('drawSquare')">
          <i class="fa fa-square-o"></i></button
        ><br />
        Shapes
      </div>
      <div class="colorcontainer border">
        <button class="btncolor black" @click="setColor('black')"></button>
        <button
          class="btncolor darkgrey"
          @click="setColor('darkgrey')"
        ></button>
        <button class="btncolor darkred" @click="setColor('darkred')"></button>
        <button class="btncolor red" @click="setColor('red')"></button>
        <button class="btncolor orange" @click="setColor('orange')"></button>
        <button class="btncolor yellow" @click="setColor('yellow')"></button>
        <button class="btncolor green" @click="setColor('green')"></button>
        <button
          class="btncolor turquoise"
          @click="setColor('turquoise')"
        ></button>
        <button class="btncolor indigo" @click="setColor('indigo')"></button>
        <button class="btncolor purple" @click="setColor('purple')"></button
        ><br />
        <button class="btncolor white" @click="setColor('white')"></button>
        <button
          class="btncolor lightgrey"
          @click="setColor('lightgrey')"
        ></button>
        <button class="btncolor brown" @click="setColor('brown')"></button>
        <button
          class="btncolor rose"
          @click="setColor(' rgb(229, 57, 235)')"
        ></button>
        <button class="btncolor gold" @click="setColor('gold')"></button>
        <button
          class="btncolor lightyellow"
          @click="setColor('lightyellow')"
        ></button>
        <button class="btncolor lime" @click="setColor('lime')"></button>
        <button
          class="btncolor ligtturquoise"
          @click="setColor(' rgb(133, 233, 223)')"
        ></button>
        <button class="btncolor blue" @click="setColor('blue')"></button>
        <button class="btncolor lavender" @click="setColor('lavender')"></button
        ><br />
        <button class="btncolordisable" disabled></button>
        <button class="btncolordisable" disabled></button>
        <button class="btncolordisable" disabled></button>
        <button class="btncolordisable" disabled></button>
        <button class="btncolordisable" disabled></button>
        <button class="btncolordisable" disabled></button>
        <button class="btncolordisable" disabled></button>
        <button class="btncolordisable" disabled></button>
        <button class="btncolordisable" disabled></button>
        <button class="btncolordisable" disabled></button><br />
        Colours
      </div>
    </div>
    <canvas @click="checkClick" id="canvas"> </canvas>
  </div>
</template>

<script>
export default {
  name: "Canvas",
  data() {
    return {
      canvas: null,
      ctx: null,
      running: "Nothing",
      point: "stop",
      pos1: null,
      pos2: null,
      pos3: null,
      color: "white",
      rectanagles: [],
      circles: [],
      ellipses: [],
      triangles: [],
      squeres: [],
      lines: [],
      loading: false,
      data: "",
      content: "",
      id: 0
    };
  },
  methods: {
    repaint() {
      this.ctx.clearRect(0, 0, 750, 800);
      console.log("hello");
      this.loading = true;
      for (let i = 0; i < this.rectanagles.length; i++) {
        this.drawRectangle(
          this.rectanagles[i].startPointX,
          this.rectanagles[i].startPointY,
          this.rectanagles[i].width,
          this.rectanagles[i].height,
          this.rectanagles[i].color
        );
      }
      for (let i = 0; i < this.squeres.length; i++) {
        this.drawSquare(
          this.squeres[i].startPointX,
          this.squeres[i].startPointY,
          this.squeres[i].length,
          this.squeres[i].color
        );
      }
      for (let i = 0; i < this.circles.length; i++) {
        this.drawCircle(
          this.circles[i].centerX,
          this.circles[i].centerY,
          this.circles[i].radius,
          this.circles[i].color
        );
      }
      for (let i = 0; i < this.lines.length; i++) {
        this.drawLine(
          this.lines[i].startPointX,
          this.lines[i].startPointY,
          this.lines[i].endPointX,
          this.lines[i].endPointY,
          this.lines[i].color
        );
      }
      for (let i = 0; i < this.triangles.length; i++) {
        this.drawtriangle(
          this.triangles[i].point1X,
          this.triangles[i].point1Y,
          this.triangles[i].point2X,
          this.triangles[i].point2Y,
          this.triangles[i].point3X,
          this.triangles[i].point3Y,
          this.triangles[i].color
        );
      }

      for (let i = 0; i < this.ellipses.length; i++) {
        this.drawEllipse(
          this.ellipses[i].centerX,
          this.ellipses[i].centerY,
          this.ellipses[i].radiusX,
          this.ellipses[i].radiusY,
          this.ellipses[i].color
        );
      }
      this.loading = false;
    },

    alerts(running) {
      this.point = "No Point";
      switch (running) {
        case "drawLine":
          alert("choose two points represent the line (by mouse click!!)");
          break;
        case "drawCircle":
          alert(
            "choose two points represent the center and a point on the circle (by mouse click!!)"
          );
          break;
        case "drawRectangle":
          alert(
            "choose two points represent the diagonal of the rectangle (by mouse click!!)"
          );
          break;
        case "drawSquare":
          alert(
            "choose two points represent the diagonal of the square(by mouse click!!)"
          );
          break;
        case "drawTriangle":
          alert(
            "choose three points represents the triangle (by mouse click!!)"
          );
          break;
        case "drawEllipse":
          alert(
            "choose two points on the surface of the ellipse represent the first quarter (by mouse click!!)"
          );
          break;
        case "copy":
          alert(
            "choose the shape you want to copy then choose where you want to copy it (by mouse click!!)"
          );
          break;
        case "move":
          alert(
            "choose the shape you want to move then choose where you want to move it (by mouse click!!)"
          );
          break;
        case "delete":
          alert("choose the shape you want to delete (by mouse click!!)");
          break;
        case "resize":
          alert(
            "choose the shape you want to resize then enter the scalar ratio (by mouse click!!)"
          );
          break;
      }
      this.running = running;
    },
    checkClick(e) {
      var rect = this.canvas.getBoundingClientRect();
      var mousex = e.clientX - rect.left; //Position of mouse in x-axis from canvasRectangle from left
      var mousey = e.clientY - rect.top; //Position of mouse in y-axis from canvasRectangle from top
      const pos = { x: mousex, y: mousey };
      switch (this.point) {
        case "No Point":
          this.pos1 = pos;
          this.point = "first";
          this.OneClick(this.pos1);
          break;
        case "first":
          this.pos2 = pos;
          this.point = "second";
          this.TwoClicks(this.pos1, this.pos2);
          break;
        case "second":
          this.pos3 = pos;
          this.point = "No point";
          this.ThreeClicks(this.pos1, this.pos2, this.pos3);
          break;
      }
    },
    OneClick(pos1) {
      switch (this.running) {
        case "delete":
          this.delete(pos1);
          this.running = "Nothing";
          this.point = "No Point";
          break;
        case "resize":
          this.resize(pos1);
          this.running = "Nothing";
          this.point = "No Point";
          break;
      }
    },
    TwoClicks(pos1, pos2) {
      var start, end;
      switch (this.running) {
        case "drawLine":
          this.drawLine(pos1.x, pos1.y, pos2.x, pos2.y, this.color);
          this.running = "Nothing";
          this.point = "No Point";
          break;
        case "drawCircle":
          this.drawCircle(
            pos1.x,
            pos1.y,
            Math.sqrt(
              Math.pow(pos2.y - pos1.y, 2) + Math.pow(pos2.x - pos1.x, 2)
            ),
            this.color
          );
          this.running = "Nothing";
          this.point = "No Point";
          break;
        case "drawRectangle":
          if (pos2.x > pos1.x) {
            start = pos1;
            end = pos2;
          } else {
            start = pos2;
            end = pos1;
          }
          this.drawRectangle(
            start.x,
            start.y,
            Math.abs(end.x - start.x),
            Math.abs(end.y - start.y),
            this.color
          );
          this.running = "Nothing";
          this.point = "No Point";
          break;
        case "drawSquare":
          //var start,end;
          if (pos2.x > pos1.x) {
            start = pos1;
            end = pos2;
          } else {
            start = pos2;
            end = pos1;
          }
          this.drawSquare(
            start.x,
            start.y,
            Math.max(Math.abs(end.x - start.x), Math.abs(end.y - start.y)),
            this.color
          );
          this.running = "Nothing";
          this.point = "No Point";
          break;
        case "drawEllipse":
          var posx = null,
            posy = null;
          if (pos2.x < pos1.x) {
            posx = pos1;
            posy = pos2;
          } else {
            posx = pos2;
            posy = pos1;
          }
          this.drawEllipse(
            posy.x,
            posx.y,
            Math.abs(posx.x - posy.x),
            Math.abs(posy.y - posx.y),
            this.color
          );
          this.running = "Nothing";
          this.point = "No Point";
          break;
        case "move":
          this.move(pos1, pos2);
          this.running = "Nothing";
          this.point = "No Point";
          break;
        case "copy":
          this.copy(pos1, pos2);
          this.running = "Nothing";
          this.point = "No Point";
          break;
      }
    },
    ThreeClicks(pos1, pos2, pos3) {
      switch (this.running) {
        case "drawTriangle":
          this.drawtriangle(
            pos1.x,
            pos1.y,
            pos2.x,
            pos2.y,
            pos3.x,
            pos3.y,
            this.color
          );
          this.running = "Nothing";
          this.point = "No Point";
          break;
      }
    },
    drawLine(point1x, point1y, point2x, point2y, color) {
      if (!this.loading) {
        this.id++;
        const a = {
          name: "LineSegment",
          startPointX: point1x,
          startPointY: point1y,
          endPointX: point2x,
          endPointY: point2y,
          color: color,
          id: this.id + ""
        };
        //send the data to back end and notify that's a line
        this.backEnd(a);
      }
      //var ctx = this.canvas.getContext("2d");
      this.ctx.beginPath();
      this.ctx.moveTo(point1x, point1y);
      this.ctx.lineTo(point2x, point2y);
      this.ctx.strokeStyle = color;
      this.ctx.stroke();
    },
    drawCircle(pointx, pointy, radius, color) {
      if (!this.loading) {
        this.id++;
        const a = {
          name: "Circle",
          centerX: pointx,
          centerY: pointy,
          radius: radius,
          color: color,
          id: this.id + ""
        };
        //send the data to back end and notify that's a circle
        this.backEnd(a);
      }
      this.ctx.beginPath();
      this.ctx.arc(pointx, pointy, radius, 0, 2 * Math.PI);
      this.ctx.strokeStyle = "black";
      this.ctx.fillStyle = color;
      this.ctx.fill();
      this.ctx.stroke();
    },
    drawRectangle(point1x, point1y, width, height, color) {
      if (!this.loading) {
        this.id++;
        const a = {
          name: "Rectangle",
          startPointX: point1x,
          startPointY: point1y,
          width: width,
          height: height,
          color: color,
          id: this.id + ""
        };
        //send the data to back end and notify that's a rectangle
        this.backEnd(a);
      }
      this.ctx.beginPath();
      this.ctx.fillStyle = color;
      this.ctx.fillRect(point1x, point1y, width, height);
      this.ctx.strokeStyle = "black";
      this.ctx.strokeRect(point1x, point1y, width, height);
    },
    drawSquare(point1x, point1y, long, color) {
      if (!this.loading) {
        this.id++;
        const a = {
          name: "Square",
          startPointX: point1x,
          startPointY: point1y,
          length: long,
          color: color,
          id: this.id + ""
        };
        //send the data to back end and notify that's a square
        this.backEnd(a);
      }
      this.ctx.beginPath();
      this.ctx.fillStyle = color;
      this.ctx.fillRect(point1x, point1y, long, long);
      this.ctx.strokeStyle = "black";
      this.ctx.strokeRect(point1x, point1y, long, long);
    },
    drawtriangle(point1x, point1y, point2x, point2y, point3x, point3y, color) {
      if (!this.loading) {
        this.id++;
        const a = {
          name: "Triangle",
          point1X: point1x,
          point1Y: point1y,
          point2X: point2x,
          point2Y: point2y,
          point3X: point3x,
          point3Y: point3y,
          color: color,
          id: this.id + ""
        };
        //send the data to back end and notify that's a triangle
        this.backEnd(a);
      }
      this.ctx.beginPath();
      this.ctx.moveTo(point1x, point1y);
      this.ctx.lineTo(point2x, point2y);
      this.ctx.lineTo(point3x, point3y);
      this.ctx.closePath();
      this.ctx.strokeStyle = "black";
      this.ctx.fillStyle = color;
      this.ctx.fill();
      this.ctx.stroke();
    },
    drawEllipse(x, y, radiusX, radiusY, color) {
      if (!this.loading) {
        this.id++;
        const a = {
          name: "Ellipse",
          centerX: x,
          centerY: y,
          radiusX: radiusX,
          radiusY: radiusY,
          color: color,
          id: this.id + ""
        };
        //send the data to back end and notify that's an ellipse
        this.backEnd(a);
      }
      this.ctx.beginPath();
      this.ctx.ellipse(x, y, radiusX, radiusY, 0, 0, 2 * Math.PI);
      this.ctx.strokeStyle = "black";
      this.ctx.fillStyle = color;
      this.ctx.fill();
      this.ctx.stroke();
    },
    delete(pos1) {
      console.log("Hey i'm in delete");
      console.log(pos1);
      fetch("http://localhost:8081//delete", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(pos1)
      })
        .then(respose => respose)
        .then(data => {
          data;
          this.loadBack();
        });
    },
    move(pos1, pos2) {
      this.id++;
      let a = {
        pos1: pos1,
        pos2: pos2
      };
      console.log("Hey i'm in move");
      console.log(pos1);
      console.log(pos2);
      fetch("http://localhost:8081//move", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(a)
      })
        .then(respose => respose)
        .then(data => {
          data;
          this.loadBack();
        });
    },
    copy(pos1, pos2) {
      this.id++;
      let a = {
        pos1: pos1,
        pos2: pos2
      };
      console.log("Hey i'm in copy");
      console.log(pos1);
      console.log(pos2);
      fetch("http://localhost:8081//copy", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(a)
      })
        .then(respose => respose)
        .then(data => {
          data;
          this.loadBack();
        });
    },
    resize(pos1) {
      this.id++;
      var Scalar = prompt("Enter the scalar ratio ", 1.0);
      let a = {
        pos1: pos1,
        Scalar: Scalar
      };
      fetch("http://localhost:8081//resize", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(a)
      })
        .then(respose => respose)
        .then(data => {
          data;
          this.loadBack();
        });
      //this.loadBack();
    },
    loadBack() {
      let a = [];
      console.log("Hey i'm in load");
      fetch("http://localhost:8081//load")
        .then(response => response.text())
        .then(data => {
          a = JSON.parse(data);
          this.handle(a);
          this.repaint();
        });
    },
    handle(a) {
      let a1 = [],
        a2 = [],
        a3 = [],
        a4 = [],
        a5 = [],
        a6 = [],
        id = 0;

      for (let i = 0; i < a.length; i++) {
        if (a[i].name == "Rectangle") a1.push(a[i]);
        else if (a[i].name == "Square") a2.push(a[i]);
        else if (a[i].name == "Circle") a3.push(a[i]);
        else if (a[i].name == "Ellipse") a4.push(a[i]);
        else if (a[i].name == "LineSegment") a5.push(a[i]);
        else if (a[i].name == "Triangle") a6.push(a[i]);
        else if (a[i].name == "id") id = parseInt(a[i].value);
      }

      this.rectanagles = a1;
      /*for (let i = 0; i < this.rectanagles.length; i++)
        console.log(this.rectanagles[i].startPointX);*/
      this.squeres = a2;
      this.circles = a3;
      this.ellipses = a4;
      this.lines = a5;
      /* for (let i = 0; i < this.lines.length; i++){
        console.log(this.lines[i].startPointX);
        console.log(this.lines[i].startPointY);
        console.log(this.lines[i].endPointX);
        console.log(this.lines[i].en);
      }*/
      this.triangles = a6;
      this.id = id + 1;
      console.log(this.rectanagles);
      console.log(this.squeres);
      console.log(this.circles);
      console.log(this.ellipses);
      console.log(this.triangles);
      console.log(this.lines);
      console.log("\n\nhelloAll=>" + this.id);
    },
    undo() {
      let a = [];
      //console.log("Hey i'm in undo");
      fetch("http://localhost:8081//undo")
        .then(response => response.text())
        .then(data => {
          a = JSON.parse(data);
          this.handle(a);
          this.repaint();
        });
    },
    redo() {
      let a = [];
      console.log("Hey i'm in redo");
      fetch("http://localhost:8081//redo")
        .then(response => response.text())
        .then(data => {
          a = JSON.parse(data);
          this.handle(a);
          this.repaint();
        });
    },
    save() {
      console.log("Hey i'm in save");
      var filename = prompt("Enter File Name ", "new file");
      //this.data = this.dataToString()+"";
      // this.dataToString();
      if (filename !== null) {
        var filetype = "";
        if (
          confirm(
            "Are you want to sava as a json file?\n if you cancelled the file will saved as a xml file!"
          )
        ) {
          filetype = "application/json";
        } else {
          filetype = "application/xml";
        }
        console.log(this.data + "1");
        //var data = "hello";
        var file = new Blob([this.data], { type: filetype });
        if (window.navigator.msSaveOrOpenBlob)
          // IE10+
          window.navigator.msSaveOrOpenBlob(file, filename);
        else {
          // Others
          var a = document.createElement("a"),
            url = URL.createObjectURL(file);
          a.href = url;
          a.download = filename;
          document.body.appendChild(a);
          a.click();
          setTimeout(function() {
            document.body.removeChild(a);
            window.URL.revokeObjectURL(url);
          }, 0);
        }
      }
    },
    load() {
      console.log("Hey i'm in load");
      var vm = this;
      const file = event.target.files[0];
      const reader = new FileReader();
      //reader.onload = e => this.$emit("load", e.target.result);
      reader.onload = function(event) {
        var contents = event.target.result;
        console.log("load " + contents);
        let a = [];
        a = JSON.parse(contents);
        vm.handle(a);
        vm.repaint();
        fetch("http://localhost:8081//loading", {
          method: "POST",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify(JSON.parse(contents))
        })
          .then(response => response)
          .then(data => data);
      };
      reader.readAsText(file);
    },

    dataToString() {
      console.log("Hey i'm in load");
      fetch("http://localhost:8081//load")
        .then(response => response.text())
        .then(data => {
          this.data = data;
          console.log(this.data);
          this.save();
        });
    },
    setColor(color) {
      this.color = color;
    },
    backEnd(a) {
      fetch("http://localhost:8081//create", {
        method: "POST",
        headers: {
          "Content-Type": "application/json"
        },
        body: JSON.stringify(a)
      })
        .then(response => response)
        .then(data => data);
    }
  },
  mounted() {
    this.canvas = document.getElementById("canvas");
    this.canvas.height = 800;
    this.canvas.width = 750;
    this.ctx = this.canvas.getContext("2d");
  }
};
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
#canvas {
  border: 1px solid black;
  background-color: white;
  width: 750px;
  height: 800px;
  cursor: crosshair;
}
.flex-container {
  display: flex;
  flex-wrap: wrap;
  //justify-content: space-around;
  justify-content: center;
}
/*clipPath.btn{
  font-size: 15px;
  background-color: DodgerBlue;
  border: none;
  color: white;
}*/
h1 {
  color: white;
}
.input {
  display: none;
}
.btnshape {
  background-color: DodgerBlue; /* Blue background */
  border: none; /* Remove borders */
  color: white; /* White text */
  padding: 10px 14px; /* Some padding */
  font-size: 14px; /* Set a font size */
  cursor: pointer; /* Mouse pointer on hover */
}
.btnshape:hover {
  background-color: RoyalBlue;
}
.btncolor {
  border: 0.5px solid lightgray;
  padding: 10px 10px;
  font-size: 10px;
  cursor: pointer;
  //margin: 0%;
}
.btncolor:hover {
  border: 0.5px solid black;
}
.btncolordisable {
  border: 0.5px solid lightgray;
  padding: 10px 10px;
  font-size: 10px;
}
.border {
  border: 1px solid black;
  color: white;
}
/*.colorcontainer{
  background-color: white;

}*/
.red {
  background-color: red;
}
.blue {
  background-color: blue;
}
.green {
  background-color: green;
}
.black {
  background-color: black;
}
.yellow {
  background-color: yellow;
}
.darkgrey {
  background-color: darkgrey;
}
.darkred {
  background-color: darkred;
}
.orange {
  background-color: orange;
}
.turquoise {
  background-color: turquoise;
}
.indigo {
  background-color: indigo;
}
.purple {
  background-color: purple;
}
.white {
  background-color: white;
}
.lightgrey {
  background-color: lightgrey;
}
.brown {
  background-color: brown;
}
.rose {
  background-color: rgb(229, 57, 235);
}
.gold {
  background-color: gold;
}
.lightyellow {
  background-color: lightyellow;
}
.lime {
  background-color: lime;
}
.lightturquoise {
  background-color: rgb(133, 233, 223);
}
.lavender {
  background-color: lavender;
}
</style>
