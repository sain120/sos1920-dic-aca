<script>

let ecstats = [];

async function loadGraph(){

const resECStats = await fetch("/api/v1/ec-stats");

ecstats = await resECStats.json();

var MyData3 = [];

    const colors = ['rgba(83, 83, 223, .7)','rgba(83, 223, 83, .7)',
    'rgba(223, 83, 83, .7)','rgba(42, 83, 223, .7)',
    'rgba(150, 42, 223, .7)','rgba(83, 150, 42, .7)',
    'rgba(10, 223, 223, .7)'];
    var ncolor = 0;

    ecstats.forEach(ecstat => {
        MyData3.push({
            name: ecstat.country + " " + ecstat.year,
            color: colors[ncolor%(ncolor.length)],
            data: [[parseFloat(ecstat.ecu), parseFloat(ecstat.cdepc)]]
        })
        ncolor++;
    });

Highcharts.chart('container', {
    chart: {
        type: 'scatter',
        zoomType: 'xy'
    },
    title: {
        text: 'Uso de coches electricos vs Emisiones de CO2 por país'
    },
    subtitle: {
        text: 'Fuente: Wikipedia'
    },
    xAxis: {
        title: {
            enabled: true,
            text: 'Uso de coches eléctricos (%)'
        },
        startOnTick: true,
        endOnTick: true,
        showLastLabel: true
    },
    yAxis: {
        title: {
            text: 'Emisiones de C02 per cápita (tons)'
        }
    },
    legend: {
        layout: 'vertical',
        align: 'left',
        verticalAlign: 'top',
        x: 100,
        y: 70,
        floating: true,
        backgroundColor: Highcharts.defaultOptions.chart.backgroundColor,
        borderWidth: 1
    },
    plotOptions: {
        scatter: {
            marker: {
                radius: 5,
                states: {
                    hover: {
                        enabled: true,
                        lineColor: 'rgb(100,100,100)'
                    }
                }
            },
            states: {
                hover: {
                    marker: {
                        enabled: false
                    }
                }
            },
            tooltip: {
                headerFormat: '<b>{series.name}</b><br>',
                pointFormat: '{point.x} %, {point.y} tons'
            }
        }
    },
    series: MyData3
});

}
</script>

<svelte:head>
    <script src="https://code.highcharts.com/highcharts.js"></script>
	<script src="https://code.highcharts.com/modules/series-label.js"></script>
	<script src="https://code.highcharts.com/modules/exporting.js"></script>
	<script src="https://code.highcharts.com/modules/export-data.js"></script>
	<script src="https://code.highcharts.com/modules/accessibility.js" on:load="{loadGraph}"></script>
</svelte:head>

<main>
    <h2>Uso de coches electricos y emisiones de CO2</h2>
    <figure class="highcharts-figure">
        <div id="container"></div>
        <p class="highcharts-description">
            Nube de puntos mostrando la relación entre el uso de coches eléctricos y las emisiones de CO2 per cápita
    </figure>
    <form method="get" action="/">
        <button type="submit">Atrás</button>
    </form>
</main>