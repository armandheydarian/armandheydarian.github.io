# armandheydarian.github.io public
Top Military, Healthcare and Education Spending by Nation 
<html lang='en'>
<head>
<script src="https://www.gstatic.com/charts/loader.js">
//imports a library
</script>
<script>
// load that corechart 
google.charts.load('current', {'packages':['corechart']});
// immediately run the drawAllSheets function
google.charts.setOnLoadCallback(drawAllSheets);

//this is the drawallsheets function
function drawAllSheets() {
	//references drawSheetName, params (sheetname, query, responsehandler)
	drawSheetName('Copy of Education Spending 11-16.csv', 'SELECT A,B,C,D',
	militarySpendingResponseHandler);
	drawSheetName('Copy of Education Spending 11-16.csv', 'SELECT A,M,N,O',
	militarySpendingResponseHandler1);
	drawSheetName('Copy of Education Spending 11-16.csv', 'SELECT A,P,Q,R',
	militarySpendingResponseHandler2);
	drawSheetName('Copy of Education Spending 11-16.csv', 'SELECT A,S,T,U',
	militarySpendingResponseHandler3);
	drawSheetName('Copy of Education Spending 11-16.csv', 'SELECT A,V,W,X',
	militarySpendingResponseHandler4);
	drawSheetName('Copy of Education Spending 11-16.csv', 'SELECT L,D',
	topMilitarySpendingResponseHandler);
	drawSheetName('Copy of Education Spending 11-16.csv', 'SELECT A,H,I',
	averageSpendingResponseHandler);
	drawSheetName('Copy of Education Spending 11-16.csv', 'SELECT A,G,H',
	educationSpendingResponseHandler);
	drawSheetName('Copy of Education Spending 11-16.csv', 'SELECT A,H,I',
	educationSpendingResponseHandler1);
	drawSheetName('Copy of Education Spending 11-16.csv', 'SELECT A,H,J',
	educationSpendingResponseHandler2);
	drawSheetName('Copy of Education Spending 11-16.csv', 'SELECT A,H,AH',
	educationSpendingResponseHandler3);
	drawSheetName('Copy of Education Spending 11-16.csv', 'SELECT A,H,AI',
	educationSpendingResponseHandler4);
	drawSheetName('Copy of Education Spending 11-16.csv', 'SELECT A,H,AJ',
	educationSpendingResponseHandler5);
	drawSheetName('Copy of Education Spending 11-16.csv', 'SELECT A,H,AE',
	educationSpendingResponseHandler6);
	drawSheetName('Copy of Education Spending 11-16.csv', 'SELECT A,H,AF',
	educationSpendingResponseHandler7);
	drawSheetName('Copy of Education Spending 11-16.csv', 'SELECT A,H,AG',
	educationSpendingResponseHandler8);
	drawSheetName('Copy of Education Spending 11-16.csv', 'SELECT A,H,AB',
	educationSpendingResponseHandler9);
	drawSheetName('Copy of Education Spending 11-16.csv', 'SELECT A,H,AC',
	educationSpendingResponseHandler10);
	drawSheetName('Copy of Education Spending 11-16.csv', 'SELECT A,H,AD',
	educationSpendingResponseHandler11);
	drawSheetName('Copy of Education Spending 11-16.csv', 'SELECT A,H,Y',
	educationSpendingResponseHandler12);
	drawSheetName('Copy of Education Spending 11-16.csv', 'SELECT A,H,Z',
	educationSpendingResponseHandler13);
	drawSheetName('Copy of Education Spending 11-16.csv', 'SELECT A,H,AA',
	educationSpendingResponseHandler14);
	drawSheetName('Copy of Education Spending 11-16.csv', 'SELECT A,B,C',
	fastestSpendingResponseHandler);
	drawSheetName('Copy of Education Spending 11-16.csv', 'SELECT A,AK,AL',
	fastestSpendingPercentageResponseHandler);


	drawSheetName('Copy of Education Spending 11-16.csv', 'SELECT A,B,C,D,E',
	militaryOverallSpendingResponseHandler);

} //drawAllSheets

function drawSheetName(sheetName, query, responseHandler) {
	// encodes query as uri
	var queryString = encodeURIComponent(query);
	// turns query into that google can use
	//var query = new google.visualization.Query(
	// 'https://docs.google.com/spreadsheets/d/1-NwogHiqjRPfIEeh7XxgDYj1CIjyzrz7hxYus4I2VQY/gviz/tq?sheet=' + sheetName + '&headers=1&tq=' +queryString  
	//);
	var query = new google.visualization.Query(

	'https://docs.google.com/spreadsheets/d/1cThOkaAG-Q5D_rV1tMIs7eER3P4VXy_7SXSn8RQ22sk/gviz/tq?sheet=' + sheetName + '&headers=1&tq=' +queryString  
	);
	// sends query to google using one of the below responsehandlers
	query.send(responseHandler);
} //drawSheetName

function educationSpendingResponseHandler(response){
	//get the data
	var data = response.getDataTable();
	data.sort({column: 2, desc:true});
		data.setColumnLabel(1,'Per Person GDP');
	    data.setColumnLabel(2,'Per Person Education');


	//set options
	var options = {
		title: 'Per Person Education Spending vs Per Person GDP - 2014',
		height: 400,
		 vAxis: {title: 'Spending in Billions ($)'},
		 hAxis: {title: 'Country'}
	};

	//create the chart and draw it
	var chart = new google.visualization.BarChart(document.getElementById('education_spending_div'));
	chart.draw(data, options);

} //educationSpendingResponseHandler


function fastestSpendingResponseHandler(response){
	//get the data
	var data = response.getDataTable();
	data.sort({column: 2, desc:true});
		 data.setColumnLabel(1,'Education');
	     data.setColumnLabel(2,'Healthcare');


	//set options
	var options = {
		title: 'Fastest Growing Countries by Healthcare & Education Spending',
		interpolateNulls: true,
		height: 400,
	 vAxis: {title: 'Spending in Billions ($)'},
	 hAxis: {title: 'Country'}};

	//create the chart and draw it
	var chart = new google.visualization.LineChart(document.getElementById('fastest_spending_div'));
	chart.draw(data, options);

} //educationSpendingResponseHandler



function fastestSpendingPercentageResponseHandler(response){
	//get the data
	var data = response.getDataTable();
	data.sort({column: 1, desc:true});
		 data.setColumnLabel(1,'Education');
	     data.setColumnLabel(2,'Healthcare');


	//set options
	var options = {
		title: 'Fastest Growing Countries by Healthcare & Education Spending %',
		interpolateNulls: true,
		height: 400,
	 vAxis: {title: 'Spending in Billions ($)'},
	 hAxis: {title: 'Country'}};

	//create the chart and draw it
	var chart = new google.visualization.LineChart(document.getElementById('fastest_spending_p_div'));
	chart.draw(data, options);

} //educationSpendingResponseHandler

function militarySpendingResponseHandler(response){
	//get the data
	var data = response.getDataTable();
	data.sort({column: 2, desc:true});
	      data.setColumnLabel(1,'Education');
	      data.setColumnLabel(2,'Healthcare');
	      data.setColumnLabel(3,'Military');


	//set options
	var options = {
		title: 'Comparison of Military, Health & Education Spending - 2014',
		legend: {textStyle: {fontSize: 10}},
		height: 400,
	 vAxis: {title: 'Spending in Billions ($)'},
	 hAxis: {title: 'Country'}};

	//create the chart and draw it
	var chart = new google.visualization.ColumnChart(document.getElementById('military_spending_div'));
	chart.draw(data, options);

} //educationSpendingResponseHandler



function militaryOverallSpendingResponseHandler(response){
	//get the data
	var data = response.getDataTable();
	data.sort({column: 1, desc:true});
	      data.setColumnLabel(1,'Education');
	      data.setColumnLabel(2,'Healthcare');
	      data.setColumnLabel(3,'Military');
	      data.setColumnLabel(4,'GDP');


	//set options
	var options = {
		title: 'Overall Spending of Military, Healthcare, Education, Compared to GDP',
		legend: {textStyle: {fontSize: 10}},
		height: 400,
	 vAxis: {title: 'Spending in Billions ($)'},
	 hAxis: {title: 'Country'}};

	//create the chart and draw it
	var chart = new google.visualization.ColumnChart(document.getElementById('military_overall_spending_div'));
	chart.draw(data, options);

} //educationSpendingResponseHandler


function militarySpendingResponseHandler1(response){
	//get the data
	var data = response.getDataTable();
	data.sort({column: 2, desc:true});
	      data.setColumnLabel(1,'Education');
	      data.setColumnLabel(2,'Healthcare');
	      data.setColumnLabel(3,'Military');

	   
	//set options
	var options = {
		title: 'Comparison of Military, Health & Education Spending - 2015',
		legend: {textStyle: {fontSize: 10}},
		height: 400,
	 vAxis: {title: 'Spending in Billions ($)'},
	 hAxis: {title: 'Country'}};

	//create the chart and draw it
	var chart = new google.visualization.ColumnChart(document.getElementById('military_spending_div1'));
	chart.draw(data, options);
} //militarySpendingResponseHandler

//educationSpendingResponseHandler
function militarySpendingResponseHandler2(response){
	//get the data
	var data = response.getDataTable();
	data.sort({column: 2, desc:true});
	      data.setColumnLabel(1,'Education');
	      data.setColumnLabel(2,'Healthcare');
	      data.setColumnLabel(3,'Military');

	   
	//set options
	var options = {
		title: 'Comparison of Military, Health & Education Spending - 2013',
		legend: {textStyle: {fontSize: 10}},
		height: 400,
	 vAxis: {title: 'Spending in Billions ($)'},
	 hAxis: {title: 'Country'}};

	//create the chart and draw it
	var chart = new google.visualization.ColumnChart(document.getElementById('military_spending_div2'));
	chart.draw(data, options);
}

function militarySpendingResponseHandler3(response){
	//get the data
	var data = response.getDataTable();
	data.sort({column: 2, desc:true});
	      data.setColumnLabel(1,'Education');
	      data.setColumnLabel(2,'Healthcare');
	      data.setColumnLabel(3,'Military');

	   
	//set options
	var options = {
		title: 'Comparison of Military, Health & Education Spending - 2012',
		legend: {textStyle: {fontSize: 10}},
		height: 400,
	 vAxis: {title: 'Spending in Billions ($)'},
	 hAxis: {title: 'Country'}};

	//create the chart and draw it
	var chart = new google.visualization.ColumnChart(document.getElementById('military_spending_div3'));
	chart.draw(data, options);
}
function militarySpendingResponseHandler4(response){
	//get the data
	var data = response.getDataTable();
	data.sort({column: 2, desc:true});
	      data.setColumnLabel(1,'Education');
	      data.setColumnLabel(2,'Healthcare');
	      data.setColumnLabel(3,'Military');

	   
	//set options
	var options = {
		title: 'Comparison of Military, Health & Education Spending - 2011',
		legend: {textStyle: {fontSize: 10}},
		height: 400,
	 vAxis: {title: 'Spending in Billions ($)'},
	 hAxis: {title: 'Country'}};

	//create the chart and draw it
	var chart = new google.visualization.ColumnChart(document.getElementById('military_spending_div4'));
	chart.draw(data, options);	
}
function topMilitarySpendingResponseHandler(response){
//get the data
	var data = response.getDataTable();

	//set the options
	var options = {
		title: 'Countries by Military Spending',
		legend: {textStyle: {fontSize: 10}},
		height: 400,
	colorAxis: {colors: ['#e7711c', '#4374e0']}, //orange to blue
	title: 'Top 10 Countries by Military, Health & Education Spending'
};

//create the chart and draw it
var chart = new google.visualization.GeoChart(document.getElementById('top_military_spending_div'));
chart.draw(data, options);


} //topMilitarySpendingResponseHandler

function averageSpendingResponseHandler(response){
	var data = response.getDataTable();
	data.sort({column: 1, desc: true});

	var options = {
	title: 'Countries by Military Spending',
	height: 400,
	legend: 'none',
	bars: 'horizontal',
	hAxis: {title: 'Spending in Billions ($)'},
	vAxis: {title: 'Country'}
};

} //drawSheetName

function educationSpendingResponseHandler1(response){
	//get the data
	var data = response.getDataTable();
	data.sort({column: 2, desc:true});
		data.setColumnLabel(1,'Per Person GDP');
	    data.setColumnLabel(2,'Per Person Healthcare');


	//set options
	var options = {
		title: 'Per Person Healthcare Spending vs Per Person GDP - 2014',
		height: 400,
		 vAxis: {title: 'Spending in Billions ($)'},
		 hAxis: {title: 'Country'}
	};

	//create the chart and draw it
	var chart = new google.visualization.BarChart(document.getElementById('education_spending_div1'));
	chart.draw(data, options);

}

//drawSheetName

function educationSpendingResponseHandler2(response){
	//get the data
	var data = response.getDataTable();
	data.sort({column: 2, desc:true});
		data.setColumnLabel(1,'Per Person GDP');
	    data.setColumnLabel(2,'Per Person Military');


	//set options
	var options = {
		title: 'Per Person Military Spending vs Per Person GDP - 2014',
		height: 400,
		 vAxis: {title: 'Spending in Billions ($)'},
		 hAxis: {title: 'Country'}
	};

	//create the chart and draw it
	var chart = new google.visualization.BarChart(document.getElementById('education_spending_div2'));
	chart.draw(data, options);
}
function educationSpendingResponseHandler3(response){
	//get the data
	var data = response.getDataTable();
	data.sort({column: 2, desc:true});
		data.setColumnLabel(1,'Per Person GDP');
	    data.setColumnLabel(2,'Per Person Education');


	//set options
	var options = {
		title: 'Per Person Education Spending vs Per Person GDP - 2011',
		height: 400,
		 vAxis: {title: 'Spending in Billions ($)'},
		 hAxis: {title: 'Country'}
	};

	//create the chart and draw it
	var chart = new google.visualization.BarChart(document.getElementById('education_spending_div3'));
	chart.draw(data, options);	

}
function educationSpendingResponseHandler4(response){
	//get the data
	var data = response.getDataTable();
	data.sort({column: 2, desc:true});
		data.setColumnLabel(1,'Per Person GDP');
	    data.setColumnLabel(2,'Per Person Education');


	//set options
	var options = {
		title: 'Per Person Healthcare Spending vs Per Person GDP - 2011',
		height: 400,
		 vAxis: {title: 'Spending in Billions ($)'},
		 hAxis: {title: 'Country'}
	};

	//create the chart and draw it
	var chart = new google.visualization.BarChart(document.getElementById('education_spending_div4'));
	chart.draw(data, options);		

}
function educationSpendingResponseHandler5(response){
	//get the data
	var data = response.getDataTable();
	data.sort({column: 2, desc:true});
		data.setColumnLabel(1,'Per Person GDP');
	    data.setColumnLabel(2,'Per Person Education');


	//set options
	var options = {
		title: 'Per Person Military Spending vs Per Person GDP - 2011',
		height: 400,
		 vAxis: {title: 'Spending in Billions ($)'},
		 hAxis: {title: 'Country'}
	};

	//create the chart and draw it
	var chart = new google.visualization.BarChart(document.getElementById('education_spending_div5'));
	chart.draw(data, options);		

}
function educationSpendingResponseHandler6(response){
	//get the data
	var data = response.getDataTable();
	data.sort({column: 2, desc:true});
		data.setColumnLabel(1,'Per Person GDP');
	    data.setColumnLabel(2,'Per Person Education');


	//set options
	var options = {
		title: 'Per Person Education Spending vs Per Person GDP - 2012',
		height: 400,
		 vAxis: {title: 'Spending in Billions ($)'},
		 hAxis: {title: 'Country'}
	};

	//create the chart and draw it
	var chart = new google.visualization.BarChart(document.getElementById('education_spending_div6'));
	chart.draw(data, options);

}
function educationSpendingResponseHandler7(response){
	//get the data
	var data = response.getDataTable();
	data.sort({column: 2, desc:true});
		data.setColumnLabel(1,'Per Person GDP');
	    data.setColumnLabel(2,'Per Person Education');


	//set options
	var options = {
		title: 'Per Person Healthcare Spending vs Per Person GDP - 2012',
		height: 400,
		 vAxis: {title: 'Spending in Billions ($)'},
		 hAxis: {title: 'Country'}
	};

	//create the chart and draw it
	var chart = new google.visualization.BarChart(document.getElementById('education_spending_div7'));
	chart.draw(data, options);	
}
function educationSpendingResponseHandler8(response){
	//get the data
	var data = response.getDataTable();
	data.sort({column: 2, desc:true});
		data.setColumnLabel(1,'Per Person GDP');
	    data.setColumnLabel(2,'Per Person Education');


	//set options
	var options = {
		title: 'Per Person Military Spending vs Per Person GDP - 2012',
		height: 400,
		 vAxis: {title: 'Spending in Billions ($)'},
		 hAxis: {title: 'Country'}
	};

	//create the chart and draw it
	var chart = new google.visualization.BarChart(document.getElementById('education_spending_div8'));
	chart.draw(data, options);	

}
function educationSpendingResponseHandler9(response){
	//get the data
	var data = response.getDataTable();
	data.sort({column: 2, desc:true});
		data.setColumnLabel(1,'Per Person GDP');
	    data.setColumnLabel(2,'Per Person Education');


	//set options
	var options = {
		title: 'Per Person Education Spending vs Per Person GDP - 2013',
		height: 400,
		 vAxis: {title: 'Spending in Billions ($)'},
		 hAxis: {title: 'Country'}
	};

	//create the chart and draw it
	var chart = new google.visualization.BarChart(document.getElementById('education_spending_div9'));
	chart.draw(data, options);	
}
function educationSpendingResponseHandler10(response){
	//get the data
	var data = response.getDataTable();
	data.sort({column: 2, desc:true});
		data.setColumnLabel(1,'Per Person GDP');
	    data.setColumnLabel(2,'Per Person Education');


	//set options
	var options = {
		title: 'Per Person Healthcare Spending vs Per Person GDP - 2013',
		height: 400,
		 vAxis: {title: 'Spending in Billions ($)'},
		 hAxis: {title: 'Country'}
	};

	//create the chart and draw it
	var chart = new google.visualization.BarChart(document.getElementById('education_spending_div10'));
	chart.draw(data, options);
}
function educationSpendingResponseHandler11(response){
	//get the data
	var data = response.getDataTable();
	data.sort({column: 2, desc:true});
		data.setColumnLabel(1,'Per Person GDP');
	    data.setColumnLabel(2,'Per Person Education');


	//set options
	var options = {
		title: 'Per Person Military Spending vs Per Person GDP - 2013',
		height: 400,
		 vAxis: {title: 'Spending in Billions ($)'},
		 hAxis: {title: 'Country'}
	};

	//create the chart and draw it
	var chart = new google.visualization.BarChart(document.getElementById('education_spending_div11'));
	chart.draw(data, options);

}
function educationSpendingResponseHandler12(response){
	//get the data
	var data = response.getDataTable();
	data.sort({column: 2, desc:true});
		data.setColumnLabel(1,'Per Person GDP');
	    data.setColumnLabel(2,'Per Person Education');


	//set options
	var options = {
		title: 'Per Person Education Spending vs Per Person GDP - 2015',
		height: 400,
		 vAxis: {title: 'Spending in Billions ($)'},
		 hAxis: {title: 'Country'}
	};

	//create the chart and draw it
	var chart = new google.visualization.BarChart(document.getElementById('education_spending_div12'));
	chart.draw(data, options);	

}
function educationSpendingResponseHandler13(response){
	//get the data
	var data = response.getDataTable();
	data.sort({column: 2, desc:true});
		data.setColumnLabel(1,'Per Person GDP');
	    data.setColumnLabel(2,'Per Person Education');


	//set options
	var options = {
		title: 'Per Person Healthcare Spending vs Per Person GDP - 2015',
		height: 400,
		 vAxis: {title: 'Spending in Billions ($)'},
		 hAxis: {title: 'Country'}
	};

	//create the chart and draw it
	var chart = new google.visualization.BarChart(document.getElementById('education_spending_div13'));
	chart.draw(data, options);	
}
function educationSpendingResponseHandler14(response){
	//get the data
	var data = response.getDataTable();
	data.sort({column: 2, desc:true});
		data.setColumnLabel(1,'Per Person GDP');
	    data.setColumnLabel(2,'Per Person Military');


	//set options
	var options = {
		title: 'Per Person Military Spending vs Per Person GDP - 2015',
		height: 400,
		 vAxis: {title: 'Spending in Billions ($)',showTextEvery:1},
		 hAxis: {title: 'Country',showTextEvery:1}
	};

	//create the chart and draw it
	var chart = new google.visualization.BarChart(document.getElementById('education_spending_div14'));
	chart.draw(data, options);				

}

/* using a view */
var view = new google.visualization.DataView(data);

view.setColumns([0, 1, {
	calc: function(dt, row){
	return Math.floor(dt.getFormattedValue(row, 1)) + 'B';
},
	sourceColumn: 1,
	type: 'string',
	role: 'annotation'
}]);

var chart = new google.visualization.BarChart(document.getElementById('average_spending_div'));
chart.draw(view, options); //passing the view instead of data

//averageSpendingResponseHandler

</script>
</head>

<body>
	<h2>Top 10 Countries by Military, Health & Education Spending</h2>
	<div id="military_spending_div4" style="width: 900px; height: 500px;"></div>
	<div id="military_spending_div3" style="width: 900px; height: 500px;"></div>
	<div id="military_spending_div2" style="width: 900px; height: 500px;"></div>
	<div id="military_spending_div" style="width: 900px; height: 500px;"></div>
	<div id="military_spending_div1" style="width: 900px; height: 500px;"></div>
	<p></p>
	<h2>Top Countries by Military Spending</h2>
	<div id="top_military_spending_div" style="width: 900px; height: 500px;"></div>
	<p></p>
	<div id="average_spending_div" style="width: 900px; height: 500px;"></div>
	<div id="fastest_spending_div" style="width: 900px; height: 500px;"></div>
	<div id="fastest_spending_p_div" style="width: 900px; height: 500px;"></div>
	<div id="military_overall_spending_div" style="width: 900px; height: 500px;"></div>

	<div id="education_spending_div3" style="width: 900px; height: 500px;"></div>
	<div id="education_spending_div4" style="width: 900px; height: 500px;"></div>
	<div id="education_spending_div5" style="width: 900px; height: 500px;"></div>
	<div id="education_spending_div6" style="width: 900px; height: 500px;"></div>
	<div id="education_spending_div7" style="width: 900px; height: 500px;"></div>
	<div id="education_spending_div8" style="width: 900px; height: 500px;"></div>
	<div id="education_spending_div9" style="width: 900px; height: 500px;"></div>
	<div id="education_spending_div10" style="width: 900px; height: 500px;"></div>
	<div id="education_spending_div11" style="width: 900px; height: 500px;"></div>
	<div id="education_spending_div" style="width: 900px; height: 500px;"></div>
	<div id="education_spending_div1" style="width: 900px; height: 500px;"></div>
	<div id="education_spending_div2" style="width: 900px; height: 500px;"></div>
	<div id="education_spending_div12" style="width: 900px; height: 500px;"></div>
	<div id="education_spending_div13" style="width: 900px; height: 500px;"></div>
	<div id="education_spending_div14" style="width: 900px; height: 500px;"></div>
</body>
</html>
