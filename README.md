# d3-test

[check it out](d3/index.html)

## another experiment

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Proin condimentum finibus sagittis. Nunc sit amet lorem orci. Nullam molestie tempus tellus, vel eleifend sapien interdum et. Integer lorem magna, pulvinar sed ultricies ut, tincidunt eu nisi. Suspendisse potenti. Suspendisse dignissim eget nisl vehicula ornare. Nullam auctor venenatis tempor. Vestibulum euismod fringilla nisl, vel accumsan mauris gravida non. Vivamus eget dolor ac dolor laoreet laoreet.

Vivamus malesuada velit sagittis dapibus vulputate. In et ligula scelerisque, egestas enim eu, porta dui. Aenean metus orci, commodo eu turpis ac, pharetra vestibulum urna. Aenean mattis risus lorem, ut lobortis mi imperdiet eget. Cras libero diam, commodo vel ullamcorper in, ultrices vitae urna. Vestibulum ut lectus vitae nisl consectetur posuere. Nulla vestibulum iaculis nunc vitae varius. In hac habitasse platea dictumst. Duis ac hendrerit quam. Nam eu nulla porttitor, vehicula quam sed, semper est. Maecenas eget dictum leo, a condimentum velit. Vestibulum pellentesque venenatis augue. Nulla iaculis sem non viverra blandit. Quisque varius sem non metus placerat facilisis. Phasellus justo risus, aliquam ut eleifend eget, mollis id odio. Nullam semper convallis ipsum ut finibus.


<!-- D3 BEGINS HERE -->
<style>

body {
  font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
  margin: auto;
  position: relative;
  width: 960px;
}

text {
  font: 10px sans-serif;
}

form {
  position: relative;
  margin: 0 auto;
}

input {
  margin: 0 7px;
}

</style>
<h1>View topics in Docs</h1>
<form></form>
<script src="http://d3js.org/d3.v3.min.js"></script>
<script>

var width = 960,
    height = 500,
    radius = Math.min(width, height) / 2;

var color = d3.scale.ordinal()
    .range(["#DF6262", "#DF9B62", "#3B8686", "#4FB34F"]);

var pie = d3.layout.pie()
    .value(function(d) { return d.count; })
    .sort(null);

var arc = d3.svg.arc()
    .innerRadius(radius - 100)
    .outerRadius(radius - 20);

var svg = d3.select("body").append("svg")
    .attr("width", width)
    .attr("height", height)
  .append("g")
    .attr("transform", "translate(" + width / 2 + "," + height / 2 + ")");

var path = svg.selectAll("path");

d3.tsv("d3/data.tsv", type, function(error, data) {
  var regionsByFruit = d3.nest()
      .key(function(d) { return d.fruit; })
      .entries(data)
      .reverse();

  var label = d3.select("form").selectAll("label")
      .data(regionsByFruit)
    .enter().append("label");

  label.append("input")
      .attr("type", "radio")
      .attr("name", "fruit")
      .attr("value", function(d) { return d.key; })
      .on("change", change)
    .filter(function(d, i) { return !i; })
      .each(change)
      .property("checked", true);

  label.append("span")
      .text(function(d) { return d.key; });

  function change(region) {
    var data0 = path.data(),
        data1 = pie(region.values);

    path = path.data(data1, key);

    path.enter().append("path")
        .each(function(d, i) { this._current = findNeighborArc(i, data0, data1, key) || d; })
        .attr("fill", function(d) { return color(d.data.region); })
      .append("title")
        .text(function(d) { return d.data.region; });

    path.exit()
        .datum(function(d, i) { return findNeighborArc(i, data1, data0, key) || d; })
      .transition()
        .duration(750)
        .attrTween("d", arcTween)
        .remove();

    path.transition()
        .duration(750)
        .attrTween("d", arcTween);
  }
});

function key(d) {
  return d.data.region;
}

function type(d) {
  d.count = +d.count;
  return d;
}

function findNeighborArc(i, data0, data1, key) {
  var d;
  return (d = findPreceding(i, data0, data1, key)) ? {startAngle: d.endAngle, endAngle: d.endAngle}
      : (d = findFollowing(i, data0, data1, key)) ? {startAngle: d.startAngle, endAngle: d.startAngle}
      : null;
}

// Find the element in data0 that joins the highest preceding element in data1.
function findPreceding(i, data0, data1, key) {
  var m = data0.length;
  while (--i >= 0) {
    var k = key(data1[i]);
    for (var j = 0; j < m; ++j) {
      if (key(data0[j]) === k) return data0[j];
    }
  }
}

// Find the element in data0 that joins the lowest following element in data1.
function findFollowing(i, data0, data1, key) {
  var n = data1.length, m = data0.length;
  while (++i < n) {
    var k = key(data1[i]);
    for (var j = 0; j < m; ++j) {
      if (key(data0[j]) === k) return data0[j];
    }
  }
}

function arcTween(d) {
  var i = d3.interpolate(this._current, d);
  this._current = i(0);
  return function(t) { return arc(i(t)); };
}

</script>

<table border=0 style="">
<tr>
<th>
&nbsp;
</th>
<th>
Topic
</th>
<th>
Color
</th>
</tr>
<tr>
<td>
1.	
</td>
<th style="text-align: left;">
graphical information
</th>
<td style = "background-color: #DF6262;">
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;
</tr>
<tr>
<td>
2.	
</td>
<th style="text-align: left;">
field of dh
</th>
<td style = "background-color: #DF9B62;">
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;
</tr>
<tr>
<td>
3.	
</td>
<th style="text-align: left;">
literary history
</th>
<td style = "background-color: #3B8686;">
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;
</tr>
<tr>
<td>
4.
</td>
<th style="text-align: left;">
cultural data
</th>
<td style = "background-color: #4FB34F;">
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp;
</td>
</tr>
<table>
  
<!-- D3 ENDS HERE -->
  
Morbi feugiat nulla nunc, at efficitur eros hendrerit a. Praesent eget sapien aliquam, molestie urna non, rutrum orci. Praesent enim neque, molestie non tristique quis, varius a nulla. Mauris sed tortor mattis, ornare lorem nec, ullamcorper est. Morbi tincidunt consequat volutpat. Phasellus sed enim a ipsum faucibus tempor. Vivamus sed tempus risus. Pellentesque ornare vulputate tincidunt. Aliquam pretium lorem eu venenatis bibendum. Nunc sollicitudin est feugiat vulputate maximus. Donec sit amet dolor quis ligula euismod tincidunt dapibus at lorem. Maecenas aliquam ornare bibendum. Aenean pulvinar consequat nibh et semper. Nunc ultricies purus quis mauris fringilla, sed laoreet orci eleifend. Curabitur dignissim pellentesque diam vitae porttitor.

Nullam sollicitudin hendrerit ultricies. Proin tempus malesuada consectetur. Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Ut libero risus, tempor vel nulla tristique, ultrices porta ex. Suspendisse congue, lectus at lobortis rhoncus, nunc turpis efficitur enim, vel elementum sapien augue at justo. In nec placerat eros. Mauris eget purus in justo dignissim rhoncus sit amet at odio. Mauris ut venenatis mauris, non aliquet velit. Pellentesque luctus semper urna, at laoreet leo fringilla quis. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus.

Maecenas ut blandit enim. Nam at lacus nisl. Phasellus condimentum nibh orci, eu malesuada enim accumsan vel. Aliquam at fringilla ligula, et gravida odio. Nunc semper sem in magna euismod, vel posuere leo eleifend. Vivamus magna augue, ullamcorper aliquet tempor in, pretium ac felis. Aliquam sed fringilla orci, eu consectetur massa.
