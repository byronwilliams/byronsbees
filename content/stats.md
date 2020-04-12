---
title: Beekeeping Statistics
date: 2020-04-12
hidden: true
---

<script src="https://cdn.jsdelivr.net/npm/chart.js@2.8.0"></script>

<canvas id="myChart"></canvas>

<script>
        var ctx = document.getElementById('myChart').getContext('2d');
var chart = new Chart(ctx, {
    // The type of chart we want to create
    type: 'line',

    // The data for our dataset
    data: {
        labels: [],
        datasets: []
    },

    // Configuration options go here
   options: {
        scales: {
            yAxes: [{
                ticks: {
                    beginAtZero: true
                }
            }]
        }
    }

});

    fetch("http://api.live.apisdex.com/api/v0/stats")
        .then((resp) => resp.json())
        .then((data) => {
            chart.data.labels = [];

const brood = {
            label: 'Brood',
            backgroundColor: 'rgb(145, 127, 97)',
            borderColor: 'rgb(255,255,255)',
            data: []
        };

const honey = {
            label: 'Honey',
            backgroundColor: 'rgb(226,214,0)',
            borderColor: 'rgb(255,255,255)',
            data: []
        };

const pollen = {
            label: 'Pollen',
            backgroundColor: 'rgb(224,110,162)',
            borderColor: 'rgb(255,255,255)',
            data: []
        };

        data.forEach((d) => {
            chart.data.labels.push(d.Date.split("T")[0]);
            brood.data.push(Math.round(d.Brood*100)/100);
            honey.data.push(Math.round(d.Honey*100)/100);
            pollen.data.push(Math.round(d.Pollen*100)/100);
        })

        chart.data.datasets = [pollen,brood,honey];


            chart.update();
        })

</script>

Notes:

- This chart is updated every time I do an inspection. 
- I look at colonies approximately every 2 weeks, though this may just be 
lifting the lid to see if an extra super needs adding
- Only frames of capped brood are counted. If I see 2.5 frames then I round down
and just say 2.
- These are aggregate numbers which include nucs, 2-6 frame mating nucs so the
full sized colonies will far exceed these numbers. For example in Apr 2020 the
average of capped brood frames is 2.37, yet the largest colonies I have are on 7
fully capped frames 