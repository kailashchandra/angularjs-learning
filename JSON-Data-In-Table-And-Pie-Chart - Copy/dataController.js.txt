var app = angular.module('ngJsonDataApp', []);
app.controller('dataController', function ($scope, $http) {
	$scope.data = [];
	$scope.objSelected = {};
	$scope.seats = [];

	//to access data from json file.
	$scope.getData = function () {
		var url = "data.json"; //Give URL which contains json file
        /*$http.get(url).success(function (response) {
			$scope.data = response;
        }).error(function(data){
			console.log("Error getting data from " + data);
		});*/

		//same data start
		$scope.data = [{
			"fname": "Ground Floor",
			"seats": [{ "seatno": "01", "status": "booked", "name": "Kailash" },
			{ "seatno": "02", "status": "available", "name": "Jagruti" },
			{ "seatno": "03", "status": "booked", "name": "Sushil" },
			{ "seatno": "04", "status": "available", "name": "Shweta" }]
		},
		{
			"fname": "First Floor",
			"seats": [{ "seatno": "01", "status": "booked", "name": "Thor" },
			{ "seatno": "02", "status": "available", "name": "Cpt America" },
			{ "seatno": "03", "status": "booked", "name": "Tony" },
			{ "seatno": "04", "status": "booked", "name": "Hulk" }]
		}];
	};
	//sample data ends.

	//get table data
	$scope.getTableData = function () {
		$scope.availableSeat = 0;
		$scope.bookedSeat = 0;
		$scope.totalSeat = 0;

		if ($scope.objSelected != undefined) {
			$scope.seats = $scope.objSelected.seats;

			//to get count
			$scope.totalSeat = $scope.seats.length;
			for (var seat of $scope.seats) {
				if (seat.status === 'booked')
					$scope.bookedSeat++;
				else if (seat.status === 'available')
					$scope.availableSeat++;
			}

			//get Chart data
			getChartData()
		}
	};

	function getChartData() {
		// Load google charts
		google.charts.load('current', { 'packages': ['corechart'] });
		google.charts.setOnLoadCallback(drawChart);

		// Draw the chart and set the chart values
		drawChart();
	}

	function drawChart() {
		if (google.visualization != undefined) {
			var data = google.visualization.arrayToDataTable([
				['Status', 'Count'],
				['Booked', $scope.bookedSeat],
				['Available', $scope.availableSeat]]);

			// Optional; add a title and set the width and height of the chart
			// Display the chart inside the <div> element with id="piechart"
			var chart = new google.visualization.PieChart(document.getElementById('piechart'));
			chart.draw(data, {
				colors: ['#e0440e', '#6ACA3D'],
				is3D: true
			});
		}
	}
});