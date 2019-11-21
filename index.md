<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Adult income</title>

    <style>
        #chart_gender {
            width: 200px;
            height: 200px;
        }
        h2 {
            font-size: 1.5rem!important;
            padding-top: 50px;
            padding-bottom: 20px;
            width: 100%;
            text-align: center;
        }
    </style>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css"
          integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">

    <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js"
            integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1"
            crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js"
            integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM"
            crossorigin="anonymous"></script>

    <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/d3/3.5.17/d3.min.js"></script>
    <script type="text/javascript" src="https://cdn.jsdelivr.net/npm/chart.js@2.9.2/dist/Chart.min.js"></script>

    <script>


        function Matrix(options) {
            var margin = {top: 50, right: 50, bottom: 100, left: 100},
                width = 728,
                height = 728,
                data = options.data,
                container = options.container,
                labelsData = options.labels,
                startColor = options.start_color,
                endColor = options.end_color;

            var widthLegend = 100;

            if (!data) {
                throw new Error('Please pass data');
            }

            if (!Array.isArray(data) || !data.length || !Array.isArray(data[0])) {
                throw new Error('It should be a 2-D array');
            }

            var maxValue = d3.max(data, function (layer) {
                return d3.max(layer, function (d) {
                    return d;
                });
            });
            var minValue = d3.min(data, function (layer) {
                return d3.min(layer, function (d) {
                    return d;
                });
            });

            var numrows = data.length;
            var numcols = data[0].length;

            var svg = d3.select(container).append("svg")
                .attr("width", width + margin.left + margin.right)
                .attr("height", height + margin.top + margin.bottom)
                .append("g")
                .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

            var background = svg.append("rect")
                .style("stroke", "black")
                .style("stroke-width", "2px")
                .attr("width", width)
                .attr("height", height);

            var x = d3.scale.ordinal()
                .domain(d3.range(numcols))
                .rangeBands([0, width]);

            var y = d3.scale.ordinal()
                .domain(d3.range(numrows))
                .rangeBands([0, height]);

            var colorMap = d3.scale.linear()
                .domain([minValue, maxValue])
                .range([startColor, endColor]);

            var row = svg.selectAll(".row")
                .data(data)
                .enter().append("g")
                .attr("class", "row")
                .attr("transform", function (d, i) {
                    return "translate(0," + y(i) + ")";
                });

            var cell = row.selectAll(".cell")
                .data(function (d) {
                    return d;
                })
                .enter().append("g")
                .attr("class", "cell")
                .attr("transform", function (d, i) {
                    return "translate(" + x(i) + ", 0)";
                });

            cell.append('rect')
                .attr("width", x.rangeBand())
                .attr("height", y.rangeBand())
                .style("stroke-width", 0);

            cell.append("text")
                .attr("dy", ".32em")
                .attr("x", x.rangeBand() / 2)
                .attr("y", y.rangeBand() / 2)
                .attr("text-anchor", "middle")
                .style("fill", function (d, i) {
                    return d >= maxValue / 2 ? 'white' : 'black';
                })
                .text(function (d, i) {
                    return d;
                });

            row.selectAll(".cell")
                .data(function (d, i) {
                    return data[i];
                })
                .style("fill", colorMap);

            var labels = svg.append('g')
                .attr('class', "labels");

            var columnLabels = labels.selectAll(".column-label")
                .data(labelsData)
                .enter().append("g")
                .attr("class", "column-label")
                .attr("transform", function (d, i) {
                    return "translate(" + x(i) + "," + height + ")";
                });

            columnLabels.append("line")
                .style("stroke", "black")
                .style("stroke-width", "1px")
                .attr("x1", x.rangeBand() / 2)
                .attr("x2", x.rangeBand() / 2)
                .attr("y1", 0)
                .attr("y2", 5);

            columnLabels.append("text")
                .attr("x", 0)
                .attr("y", y.rangeBand() / 2)
                .attr("dy", ".82em")
                .attr("text-anchor", "end")
                .attr("transform", "rotate(-60)")
                .text(function (d, i) {
                    return d;
                });

            var rowLabels = labels.selectAll(".row-label")
                .data(labelsData)
                .enter().append("g")
                .attr("class", "row-label")
                .attr("transform", function (d, i) {
                    return "translate(" + 0 + "," + y(i) + ")";
                });

            rowLabels.append("line")
                .style("stroke", "black")
                .style("stroke-width", "1px")
                .attr("x1", 0)
                .attr("x2", -5)
                .attr("y1", y.rangeBand() / 2)
                .attr("y2", y.rangeBand() / 2);

            rowLabels.append("text")
                .attr("x", -8)
                .attr("y", y.rangeBand() / 2)
                .attr("dy", ".32em")
                .attr("text-anchor", "end")
                .text(function (d, i) {
                    return d;
                });

            var key = d3.select("#legend")
                .append("svg")
                .attr("width", widthLegend)
                .attr("height", height + margin.top + margin.bottom);

            var legend = key
                .append("defs")
                .append("svg:linearGradient")
                .attr("id", "gradient")
                .attr("x1", "100%")
                .attr("y1", "0%")
                .attr("x2", "100%")
                .attr("y2", "100%")
                .attr("spreadMethod", "pad");

            legend
                .append("stop")
                .attr("offset", "0%")
                .attr("stop-color", endColor)
                .attr("stop-opacity", 1);

            legend
                .append("stop")
                .attr("offset", "100%")
                .attr("stop-color", startColor)
                .attr("stop-opacity", 1);

            key.append("rect")
                .attr("width", widthLegend / 2 - 10)
                .attr("height", height)
                .style("fill", "url(#gradient)")
                .attr("transform", "translate(0," + margin.top + ")");

            var y = d3.scale.linear()
                .range([height, 0])
                .domain([minValue, maxValue]);

            var yAxis = d3.svg.axis()
                .scale(y)
                .orient("right");

            key.append("g")
                .attr("class", "y axis")
                .attr("transform", "translate(41," + margin.top + ")")
                .call(yAxis)
        }


        $(document).ready(function () {

                let data = {
                    "labels": [
                        "age",
                        "fnlwgt",
                        "education",
                        "marital-status",
                        "occupation",
                        "race",
                        "gender",
                        "capital-gain",
                        "capital-loss",
                        "hours-per-week",
                        "income"
                    ],
                    "total_rows": 46033,
                    "gender": {
                        "male": 0.31265668187953977,
                        "female": 0.11354648434881695
                    },
                    "matrix": [
                        [
                            1,
                            -0.075756687,
                            0.037565668,
                            -0.478237588,
                            -0.029123431,
                            0.017703193,
                            -0.0816705,
                            0.079907054,
                            0.059441212,
                            0.102184835,
                            0.237337607
                        ],
                        [
                            -0.075756687,
                            1,
                            -0.040166961,
                            0.032054483,
                            0.00921208,
                            -0.106816419,
                            -0.028567269,
                            -0.004246481,
                            -0.004363126,
                            -0.018324847,
                            -0.006862488
                        ],
                        [
                            0.037565668,
                            -0.040166961,
                            1,
                            -0.021832619,
                            -0.17840205,
                            0.045604924,
                            -0.005204261,
                            0.12638245,
                            0.081463486,
                            0.145151199,
                            0.332292596
                        ],
                        [
                            -0.478237588,
                            0.032054483,
                            -0.021832619,
                            1,
                            -0.010091574,
                            -0.073056091,
                            0.274426008,
                            -0.069491873,
                            -0.066830132,
                            -0.189632034,
                            -0.369716316
                        ],
                        [
                            -0.029123431,
                            0.00921208,
                            -0.17840205,
                            -0.010091574,
                            1,
                            -0.027834707,
                            -0.066653568,
                            -0.02435242,
                            -0.014706198,
                            -0.047608651,
                            -0.053224805
                        ],
                        [
                            0.017703193,
                            -0.106816419,
                            0.045604924,
                            -0.073056091,
                            -0.027834707,
                            1,
                            -0.106517243,
                            0.015536455,
                            0.019072591,
                            0.052013914,
                            0.079489618
                        ],
                        [
                            -0.0816705,
                            -0.028567269,
                            -0.005204261,
                            0.274426008,
                            -0.066653568,
                            -0.106517243,
                            1,
                            -0.047036673,
                            -0.046266,
                            -0.230203639,
                            -0.215756124
                        ],
                        [
                            0.079907054,
                            -0.004246481,
                            0.12638245,
                            -0.069491873,
                            -0.02435242,
                            0.015536455,
                            -0.047036673,
                            1,
                            -0.032142494,
                            0.082976706,
                            0.221642702
                        ],
                        [
                            0.059441212,
                            -0.004363126,
                            0.081463486,
                            -0.066830132,
                            -0.014706198,
                            0.019072591,
                            -0.046266,
                            -0.032142494,
                            1,
                            0.055544706,
                            0.149405184
                        ],
                        [
                            0.102184835,
                            -0.018324847,
                            0.145151199,
                            -0.189632034,
                            -0.047608651,
                            0.052013914,
                            -0.230203639,
                            0.082976706,
                            0.055544706,
                            1,
                            0.226795263
                        ],
                        [
                            0.237337607,
                            -0.006862488,
                            0.332292596,
                            -0.369716316,
                            -0.053224805,
                            0.079489618,
                            -0.215756124,
                            0.221642702,
                            0.149405184,
                            0.226795263,
                            1
                        ]
                    ],
                    "marital_status": {
                        "Married-spouse-absent": 0.09777015437392796,
                        "Widowed": 0.0941358024691358,
                        "Married-civ-spouse": 0.4543377931098783,
                        "Separated": 0.06908583391486392,
                        "Divorced": 0.10403897532610404,
                        "Never-married": 0.04853781512605042,
                        "Married-AF-spouse": 0.4375
                    },
                    "race": {
                        "Black": 0.12672176308539945,
                        "Asian-Pac-Islander": 0.2832044975404076,
                        "Other": 0.12533333333333332,
                        "White": 0.26282831355846265,
                        "Amer-Indian-Eskimo": 0.12183908045977011
                    },
                    "occupation": {
                        "Male": {
                            "Tech-support": 0.3993212669683258,
                            "Adm-clerical": 0.249185667752443,
                            "Handlers-cleaners": 0.07150715071507151,
                            "Prof-specialty": 0.5603053435114503,
                            "Machine-op-inspct": 0.1550946798917944,
                            "Exec-managerial": 0.5733056708160442,
                            "Priv-house-serv": 0,
                            "Craft-repair": 0.2332008982553118,
                            "Sales": 0.3770030924936744,
                            "Transport-moving": 0.21005385996409337,
                            "Armed-Forces": 0.3333333333333333,
                            "Other-service": 0.05662921348314607,
                            "Protective-serv": 0.3403019744483159
                        },
                        "Female": {
                            "Tech-support": 0.11921708185053381,
                            "Adm-clerical": 0.08198461130273282,
                            "Handlers-cleaners": 0.031496062992125984,
                            "Prof-specialty": 0.25958965209634255,
                            "Machine-op-inspct": 0.03482587064676617,
                            "Exec-managerial": 0.2408466819221968,
                            "Priv-house-serv": 0.013157894736842105,
                            "Craft-repair": 0.1021671826625387,
                            "Sales": 0.06882383153569595,
                            "Transport-moving": 0.10236220472440945,
                            "Armed-Forces": 0.028910303928836176,
                            "Other-service": 0.12295081967213115
                        }
                    },
                    "education": {
                        "Preschool": 0.16277050494255946,
                        "1st-4th": 0.2642249836494441,
                        "5th-6th": 0.20057791948983658,
                        "7th-8th": 0.06698950766747377,
                        "9th": 0.7481481481481481,
                        "10th": 0.06872037914691943,
                        "11th": 0.4185537828100875,
                        "12th": 0.5521235521235521,
                        "HS-grad": 0.7274305555555556,
                        "Some-college": 0.053418803418803416,
                        "Assoc-voc": 0.25733063700707787,
                        "Assoc-acdm": 0.05822416302765648,
                        "Bachelors": 0.07679465776293823,
                        "Masters": 0.034934497816593885,
                        "Prof-school": 0.0136986301369863
                    }
                };


                // -- heatmap
                let correlationMatrix = data.matrix;
                let labels = data.labels;
                Matrix({
                    container: '#container',
                    data: correlationMatrix.map(item => item.map(item2 => Math.round(item2 * 10000) / 10000)),
                    labels: labels,
                    start_color: '#ffffff',
                    end_color: '#3498db'
                });

                // -- gender
                var chartGender = document.getElementById('chart_gender');
                var barChartGender = new Chart(chartGender, {
                    type: 'bar',
                    options: {
                        legend: {
                            display: false,
                            onClick: (e) => e.stopPropagation()
                        },
                        scales: {
                            yAxes: [{
                                scaleLabel:{
                                    display: true,
                                    labelString: 'Average Income',
                                    fontColor: "#546372"
                                }
                            }]
                        }
                    },
                    data: {
                        labels: Object.keys(data.gender),
                        datasets: [{
                            label: 'AVG Income',
                            data: Object.values(data.gender),
                            backgroundColor: [
                                'rgba(255, 99, 132, 0.2)',
                                'rgba(54, 162, 235, 0.2)',
                            ],
                            borderColor: [
                                'rgba(255, 99, 132, 1)',
                                'rgba(54, 162, 235, 1)',
                            ],
                            borderWidth: 1
                        }]
                    },
                });
                // -- marital status
                var chartMaritalStatus = document.getElementById('chart_marital_status');
                var barChartMaritalStatus = new Chart(chartMaritalStatus, {
                    type: 'bar',
                    options: {
                        legend: {
                            display: false,
                            onClick: (e) => e.stopPropagation()
                        },
                        scales: {
                            yAxes: [{
                                scaleLabel:{
                                    display: true,
                                    labelString: 'Average Income',
                                    fontColor: "#546372"
                                }
                            }]
                        }
                    },
                    data: {
                        labels: Object.keys(data.marital_status),
                        datasets: [{
                            label: 'AVG Income',
                            data: Object.values(data.marital_status),
                            backgroundColor: [
                                'rgba(255, 99, 132, 0.2)',
                                'rgba(54, 162, 235, 0.2)',
                                'rgba(119,235,149,0.2)',
                                'rgba(235,143,180,0.2)',
                                'rgba(235,232,142,0.2)',
                                'rgba(235,64,18,0.2)',
                                'rgba(235,29,232,0.2)',
                            ],
                            borderColor: [
                                'rgba(255, 99, 132, 1)',
                                'rgba(54, 162, 235, 1)',
                                'rgba(119,235,149, 1)',
                                'rgba(235,143,180, 1)',
                                'rgba(235,232,142, 1)',
                                'rgba(235,64,18, 1)',
                                'rgba(235,29,232, 1)',
                            ],
                            borderWidth: 1
                        }]
                    },
                });


                // -- race
                var chartRace = document.getElementById('chart_race');
                var barChartRace = new Chart(chartRace, {
                    type: 'bar',
                    options: {

                        legend: {
                            display: false,
                            onClick: (e) => e.stopPropagation()
                        },
                        scales: {
                            yAxes: [{
                                scaleLabel:{
                                    display: true,
                                    labelString: 'Average Income',
                                    fontColor: "#546372"
                                }
                            }]
                        }
                    },
                    data: {
                        labels: Object.keys(data.race),
                        datasets: [{
                            label: 'AVG Income',
                            data: Object.values(data.race),
                            backgroundColor: [
                                'rgba(255, 99, 132, 0.5)',
                                'rgba(54, 162, 235, 0.5)',
                                'rgba(119,235,149,0.5)',
                                'rgba(235,143,180,0.5)',
                                'rgba(235,232,142,0.5)',
                            ],
                            borderColor: [
                                'rgba(255, 99, 132, 1)',
                                'rgba(54, 162, 235, 1)',
                                'rgba(119,235,149, 1)',
                                'rgba(235,143,180, 1)',
                                'rgba(235,232,142, 1)',
                            ],
                            borderWidth: 1
                        }]
                    },
                });

                // -- chart_occupation
                var chartOccupation = document.getElementById('chart_occupation');
                var barChartOccupation = new Chart(chartOccupation, {
                    type: 'bar',
                    options: {
                        legend: {
                            display: false,
                            onClick: (e) => e.stopPropagation()
                        },
                        scales: {
                            xAxes: [{
                                stacked: true,
                            }],
                            yAxes: [{
                                stacked: true,
                                scaleLabel:{
                                    display: true,
                                    labelString: 'Average Income',
                                    fontColor: "#546372"
                                }
                            }]
                        }
                    },
                    data: {
                        labels: Object.keys(data.occupation.Male),
                        datasets: [{
                            label: 'Male',
                            data: Object.values(data.occupation.Male),
                            backgroundColor: 'rgba(54, 162, 235, .5)',
                            borderColor: 'rgba(54, 162, 235, 1)',
                            borderWidth: 1
                        },{
                            label: 'Female',
                            data: Object.values(data.occupation.Female),
                            backgroundColor: 'rgba(255, 99, 132, 0.5)',
                            borderColor: 'rgba(255, 99, 132, 1)',
                            borderWidth: 1
                        }]
                    }
                });



                // -- education
                var chartEducation = document.getElementById('chart_education');
                var educationLevels = Object.keys(data.education);

                var barChartEducation = new Chart(chartEducation, {
                    type: 'bar',
                    options: {

                        legend: {
                            display: false,
                            onClick: (e) => e.stopPropagation()
                        },
                        scales: {
                            yAxes: [{
                                scaleLabel:{
                                    display: true,
                                    labelString: 'Average Income',
                                    fontColor: "#546372"
                                }
                            }]
                        }
                    },
                    data: {
                        labels: educationLevels,
                        datasets: [{
                            data: Object.values(data.education),
                            backgroundColor: [
                                'rgba(255, 99, 132, 0.5)',
                                'rgba(54, 162, 235, 0.5)',
                                'rgba(119,235,149,0.5)',
                                'rgba(235,143,180,0.5)',
                                'rgba(235,232,142,0.5)',

                                'rgba(235,74,61,0.5)',
                                'rgba(140,170,167,0.5)',
                                'rgba(103,227,73,0.5)',
                                'rgba(106,4,101,0.5)',
                                'rgba(184,82,57,0.5)',

                                'rgba(45,134,51,0.5)',
                                'rgba(0,49,184,0.5)',
                                'rgba(184,123,140,0.5)',
                                'rgba(103,128,63,0.5)',
                                'rgba(15,184,129,0.5)',
                            ],
                            borderColor: [
                                'rgba(255, 99, 132, 1)',
                                'rgba(54, 162, 235, 1)',
                                'rgba(119,235,149,1)',
                                'rgba(235,143,180,1)',
                                'rgba(235,232,142,1)',

                                'rgba(235,74,61,1)',
                                'rgba(140,170,167,1)',
                                'rgba(103,227,73,1)',
                                'rgba(106,4,101,1)',
                                'rgba(184,82,57,1)',

                                'rgba(45,134,51,1)',
                                'rgba(0,49,184,1)',
                                'rgba(184,123,140,1)',
                                'rgba(103,128,63,1)',
                                'rgba(15,184,129,1)',
                            ],
                            borderWidth: 1
                        }]
                    },
                });


        });
    </script>
</head>

<body class="py-4">
<div class="container">

    <div class="row">
        <div class="col-md-12">
            <nav>
                <div class="nav nav-tabs nav-fill" id="nav-tab" role="tablist">
                    <a class="nav-item nav-link active" id="nav-heatmap-tab" data-toggle="tab" href="#nav-heatmap" role="tab" aria-controls="nav-heatmap" aria-selected="true">Heatmap</a>
                    <a class="nav-item nav-link" id="nav-gender-tab" data-toggle="tab" href="#nav-gender" role="tab" aria-controls="nav-gender" aria-selected="false">Gender</a>
                    <a class="nav-item nav-link" id="nav-marital-tab" data-toggle="tab" href="#nav-marital" role="tab" aria-controls="nav-marital" aria-selected="false">Marital status</a>
                    <a class="nav-item nav-link" id="nav-race-tab" data-toggle="tab" href="#nav-race" role="tab" aria-controls="nav-race" aria-selected="false">Race</a>
                    <a class="nav-item nav-link" id="nav-occupation-tab" data-toggle="tab" href="#nav-occupation" role="tab" aria-controls="nav-occupation" aria-selected="false">Occupation</a>
                    <a class="nav-item nav-link" id="nav-education-tab" data-toggle="tab" href="#nav-education" role="tab" aria-controls="nav-education" aria-selected="false">Education</a>
                </div>
            </nav>
            <div class="tab-content" id="nav-tabContent">
                <div class="tab-pane fade show active" id="nav-heatmap" role="tabpanel" aria-labelledby="nav-heatmap-tab">
                    <h2>Heatmap</h2>
                    <div style="display:inline-block;" id="legend"></div>
                    <div style="display:inline-block; float:left" id="container"></div>
                </div>

                <div class="tab-pane fade" id="nav-gender" role="tabpanel" aria-labelledby="nav-gender-tab">
                    <h2>Gender</h2>
                    <canvas id="chart_gender" style="position: relative; height:500px; width:100%"></canvas>
                </div>

                <div class="tab-pane fade" id="nav-marital" role="tabpanel" aria-labelledby="nav-marital-tab">
                    <h2>Marital status</h2>
                    <canvas id="chart_marital_status" style="position: relative; height:500px; width:100%"></canvas>
                </div>

                <div class="tab-pane fade" id="nav-race" role="tabpanel" aria-labelledby="nav-race-tab">
                    <h2>Race</h2>
                    <canvas id="chart_race" style="position: relative; height:500px; width:100%"></canvas>
                </div>
                <div class="tab-pane fade" id="nav-occupation" role="tabpanel" aria-labelledby="nav-race-tab">
                    <h2>Occupation</h2>
                    <canvas id="chart_occupation" style="position: relative; height:500px; width:100%"></canvas>
                </div>
                <div class="tab-pane fade" id="nav-education" role="tabpanel" aria-labelledby="nav-education-tab">
                    <h2>Education</h2>
                    <canvas id="chart_education" style="position: relative; height:500px; width:100%"></canvas>
                </div>
            </div>
        </div>
    </div>
</div> <!-- /container -->


</body>
</html>
