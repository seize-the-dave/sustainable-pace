---
title: Cost of Delay
permalink: /tools/cost-of-delay/
layout: page
---

<script src="//cdn.jsdelivr.net/jstat/latest/jstat.min.js" crossorigin="anonymous"></script>
<script type="text/javascript" src="https://www.gstatic.com/charts/loader.js"></script>
<script type="text/javascript">
  google.charts.load('current', {'packages':['corechart']});
  google.charts.setOnLoadCallback(drawChart);

  var weeks = 50;
  var work = 20;
  var income = weeks - work;
  var costPerWeek = 1000;

  function drawChart() {
    var data = new google.visualization.DataTable();
    data.addColumn('number', 'X');
    data.addColumn('number', 'Zero Delay');

    var workGraph = jStat( 0, work, work + 1, function(x) {
        return [ x, -100 ];
    })[0];
    data.addRows(workGraph);

    var gammaGraph = jStat( 0, income, income + 1, function(x) {
        return [ work + x, jStat.gamma.pdf( x, 7.5, 1.5) * 10000 ];
    })[0];
    data.addRows(gammaGraph);

    var options = {
      title: 'Company Performance',
      curveType: 'function',
      legend: { position: 'bottom' }
    };

    var chart = new google.visualization.LineChart(document.getElementById('curve_chart'));

    chart.draw(data, options);
  }
</script>
<div id="curve_chart" style="width: 900px; height: 500px"></div>
